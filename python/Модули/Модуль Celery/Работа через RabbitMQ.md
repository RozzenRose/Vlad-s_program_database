#Python #Celery #RabbitMQ

Для того, что бы **Celery** работал, нужно написать небольшую инфраструктуру. Для этого мы создаем новую папку и файлы внутри проекта:
![[Pasted image 20260109131914.png]]
В файле `celery_app.py` создается объект **Celery**, который будет называться `celery_app`. Тут же мы указываем ему `URL` на брокер сообщений и если надо **Redis** для  результата.
```python
# worker/celery_app.py
from celery import Celery
from config import settings

# Создаем Celery app с RabbitMQ как брокер
celery_app = Celery(
    'order_worker', # Имя приложения Celery
    broker=settings.rabbitmq_url,  # RabbitMQ URL
    backend=settings.redis_url,  # Redis для результатов
    include=['worker.tasks']  # Где искать задачи. Список модулей, которые Celery импортирует при старте воркера
)

# Конфигурация
celery_app.conf.update(
    task_serializer='json',
    accept_content=['json'],
    result_serializer='json',
    timezone='Europe/Moscow',
    enable_utc=True,

    # Настройки воркера
    worker_prefetch_multiplier=1,  # По одной задаче за раз
    task_acks_late=True,  # Подтверждать после выполнения
    task_reject_on_worker_lost=True,

    # Очереди
    task_default_queue='new_orders',
    task_queues={
        'new_orders': {

            'exchange': 'new_orders',
            'routing_key': 'new_order'
        }
    }
)
```

Файл `database.py` нужен, если твои воркеры хотя в БД. 
```python
# worker/database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, Session
from config import settings

# Синхронный движок для Celery
sync_engine = create_engine(
    settings.database_url_sync,  # postgresql://...
    echo=True
)

SyncSessionLocal = sessionmaker(
    sync_engine,
    autocommit=False,
    autoflush=False
)

def get_sync_db() -> Session:
    """Синхронная сессия для Celery задач"""
    db = SyncSessionLocal()
    try:
        yield db
    finally:
        db.close()
```
Прямо в нем делаем движек, причем синхронный. Тут же на месте делаем сешнмейкер и пишем функцию создания сессий.

 Движек должен быть синхронным, потому что воркеры в **Celery** синхронные. Но зато они запускаются в отдельном интерпретаторе и выполняются в отдельном потоке. Так что по сути тебе и не нужна асинхронность. 

Теперь перейдем к файлу `tasks.py`. Тут мы храним **воркеры**. Базово это просто функции. Их можно вызывать из вне через очередь сообщений, и **Celery** запустит для выполнения этой функции отдельный поток.
```python
# worker/tasks.py
import time

from celery import Celery
from app.database.models import Order  # ТА ЖЕ САМАЯ модель!
from worker.database import get_sync_db
from config import settings
from sqlalchemy import select, update
from app.database.models.orders import OrderStatus
import json
from redis_inf import Redis


celery_app = Celery(__name__, broker=settings.rabbitmq_url)


@celery_app.task
def process_order(order_id: str):
    while True:
        print('работает')
        time.sleep(60)
```
Метод `process_order()` является воркером. 

Когда в **RabbitMQ** появится сообщение для этого воркера, **Celery** запустит его.

Теперь разберемся как вызвать воркер из вне. Для начала нам нужно запустить воркер: `celery -A worker.celery_app worker`

Теперь посмотрим на пример кода, который отправит задачу в такой воркер. Это базовый ендпоинт, который создает заказ в базе данных, а заодно перекидывает данные в наш воркер для дальнейшей обработки:
```python
from worker.tasks import process_order


@router.post('/create_new_order', status_code=status.HTTP_201_CREATED)
@limiter.limit('1/second')
async def create_order(db: Annotated[AsyncSession, Depends(get_db)],
                       create_user: CreateOrder,
                       user: Annotated[dict, Depends(get_current_user)], request: Request):
    order_id = str(uuid.uuid4())
    await create_order_in_db(db, create_user, user.get('user_id'), order_id)
    process_order.apply_async(
        args=[order_id],
        queue='new_orders',
        routing_key='new_order'
    )
    return {'status_code':status.HTTP_201_CREATED,
            'transaction': 'Order created successfully'}
```