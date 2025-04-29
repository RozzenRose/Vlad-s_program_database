#Celery #Redis

![[Pasted image 20250428173747.png]]
В целом это неблокирующая очередь задач, работающая в распределенной системе. Он может управлять асинхронными фоновыми процессами, которые огромные и требуют большой нагрузки на процессор. Это сторонний инструмент, поэтому сначала нам нужно установить его:
```Power Shell
pip install Celery
```
Он планирует и выполняет задачи одновременно на одном сервере или в распределенной среде. Но для отправки и получения сообщений требуется транспорт сообщений в строках, словарях, списках, наборах, растровых изображениях и типах потоков.

В этом разделе мы будем использовать **Redis**. **Redis** прост в установку, и мы можем легко начать с него без особых проблем, установите его, следуя инструкциям [Redis Quick Start](https://redis.io/docs/getting-started/).

Лично я просто накатил **Docker** контейнер с редисом на своей виртуалке на без **Linux**:
```python
docker run -d --name redis -p 6379:6379 redis:<version>
```
Также установим зависимости в наше приложение.
```Power Shell
pip install redis
```
А потом создадим связь с **Celery/Redis** конфиги в `main.py`:
```python
import time

from celery import Celery
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

celery = Celery(
    __name__,
    broker='redis://192.168.50.196:6379/0',
    backend='redis://192.168.50.196:6379/0',
    broker_connection_retry_on_startup=True
)


def call_background_task(message):
    time.sleep(10)
    print(f"Background Task called!")
    print(message)


@app.get("/")
async def hello_world(message: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(call_background_task, message)
    return {'message': 'Hello World!'}
```
При запуске **Celery** создается 1 обработчик. Запустим в отдельной консоли его:
```power shell
celery -A main.celery worker
```
Так как версии **Celery** 4+ больше не поддерживает Windows. Для пользователей Windows сначала необходимо установить библиотеку `gevent`:
```power shell
pip install gevent
```
И для запуска необходимо использовать такую команду:
```power shell
celery -A main.celery worker --loglevel=info -P gevent
```
![[Pasted image 20250425124228.png]]
Этот обработчик выступает в роли центрального управляющего процесса (**supervisor process**), который запускает дочерние процессы или потоки для выполнения задач.

По уполчанию основной процесс будет создавать именно дочерние процессы, а не потоки. Их количество соответствует числу ядер процессора.

Центральный процесс контролирует работу задач и процессов/потоков, но не занимается непосредственным запуском задач. Группа дочерних процессов или потоков, ожидающих получения задач, называется пулом выполнения (`execution pool`) или пулом потоков (`thred pool`)

### Очереди
В **Celery** очереди (**queues**) используются для организации и управления задачами. Они позволяют группировать задачи по типу, приоритету или другим критериям, что упрощает их обработку и управление.

**Основные концепции очередей в Celery**:
- **Очередь как FIFO структура данных:** Очередь — это структура данных “первым поступил, первым обслуживается” (FIFO - First-In, First-Out). Задачи добавляются в конец очереди и извлекаются из начала.
- **Множество очередей:** В Celery можно использовать несколько очередей для организации задач. Это позволяет разделить задачи по типу или приоритету. Например, можно иметь очередь для задач высокого приоритета и очередь для задач низкого приоритета.
- **Назначение задач в очереди:** При отправке задачи можно указать, в какую очередь она должна быть помещена. Это делается с помощью аргумента  `queue` в методе `apply_async()`.
- **Рабочие и очереди:** Рабочие (workers) подключаются к определенным очередям и извлекают задачи из этих очередей. Один рабочий может подключаться к нескольким очередям.
- **Имя очереди:** Очереди имеют имена, которые используются для их идентификации. Имена очередей обычно являются строками. По умолчанию используется очередь с именем  `celery`.
- **Routers (маршрутизаторы):** Более сложные сценарии могут использовать маршрутизаторы (routers) для динамического назначения задач в очереди на основе атрибутов задачи.

**Пример использования очередей**:
```python
from celery import Celery
from celery.utils.nodenames import default_nodename

app = Celery('tasks', broker='redis://localhost', worker_hijack_root_logger=False) # Используем Redis как брокер

@app.task(queue='high_priority')
def high_priority_task():
    print("Выполнение задачи высокого приоритета")

@app.task(queue='low_priority')
def low_priority_task():
    print("Выполнение задачи низкого приоритета")


# Запуск задач
high_priority_task.apply_async()
low_priority_task.apply_async()
```
Чтобы посмотреть первые `100` задач в очереди в `Redis`, выполнение:
```PowerShell
celery -A main.app worker --loglevel=info -P gevent
```
Эти очереди сильно напоминают принцип `FIFO`, но это не совсем так. Задачи, которые сначала помещаются в очередь, первыми удаляются из очереди, но они не обязательно выполняются первыми.

### Задачи (Tasks)
В **Celery** задачи - это функции **Python**, которые выполняются асинхронно в фоновом режиме. Они являются основными единицами работы в системе **Celery**. Задачи определяются с помощью декоратора `@celery.task` и могут быть выполнены обработчиками (`workers`) из очередей (`queue`)

Прежде чем что-либо может быть запущено в **Celery**, оно должно быть декларировано как задача.
```python
import time

from celery import Celery
from fastapi import FastAPI

app = FastAPI()

celery = Celery(
    __name__,
    broker='redis://127.0.0.1:6379/0',
    backend='redis://127.0.0.1:6379/0',
    broker_connection_retry_on_startup=True
)


@celery.task
def call_background_task(message):
    time.sleep(10)
    print(f"Background Task called!")
    print(message)


@app.get("/")
async def hello_world(message: str):
    call_background_task.delay(message)
    return {'message': 'Hello World!'}
```
Обратите внимание, как мы декорировали `call_background_task` с помощью `@celery.task`. Это указывает Celery, что это задача, которая будет выполняться в очереди задач.

Основная цель экземпляра Celery — аннотировать методы Python, чтобы они стали задачами. Экземпляр Celery имеет декоратор `task()`, который мы можем применять ко всем вызываемым процедурам, которые мы хотим определить как асинхронные задачи. Частью декоратора `task()` является имя задачи, необязательное уникальное имя, состоящее из пакета, имен модулей и имени метода транзакции.

У него есть и другие атрибуты, которые могут уточнить определение задачи, например список `auto_retry`, в котором регистрируются классы исключений, которые могут вызывать повторные попытки выполнения при их создании, и `max_retries`, который ограничивает количество повторных выполнений задачи.
```python
@celery.task(name="my_first.tasks", auto_retry=[ValueError, TypeError], max_retries=5)
```

Данная задача имеет только пять повторных попыток выполнения, если во время выполнения она сталкивается с ValueError или TypeError.

Осталось только запустить. Для этого есть три способа это сделать: `apply_async`, `delay` и обычный вызов `call`.

В Celery есть три основных способа вызова задач: `apply_async`, `delay` и `call`. Хотя `call` может показаться простым вызовом функции, он имеет важные отличия от асинхронных вариантов.

**1. `apply_async()`:**
```python
call_background_task.apply_async(args=[arg1_value], kwargs={'key': 'value'})
```
Это наиболее гибкий и мощный способ вызова задач. Он позволяет управлять многими параметрами задачи, включая очередь, маршрутизацию, приоритет, и другие настройки. Вызов  `apply_async()` всегда асинхронный — он не блокирует выполнение кода.

**2. `delay()`:**
Это более компактный синтаксис для  `apply_async()`, который упрощает вызов задач с простыми аргументами. Он также всегда асинхронный.
```python
call_background_task.delay(arg1_value, arg2_value)
```

**3. `call()`:**
Это синхронный вызов задачи. Он блокирует выполнение кода до тех пор, пока задача не завершится. Он не использует брокер и рабочие процессы — задача выполняется в том же процессе, где вызывается  `call()`. Практически это просто вызов функции. В Celery этот способ редко используется, потому что он лишает всех преимуществ асинхронной обработки задач.
```python
call_background_task(arg1_value, arg2_value)
```
В большинстве случаев для вызова задач в Celery следует использовать  `apply_async()`или  `delay()`, в зависимости от сложности вашего сценария.  `call()` практически лишен преимуществ Celery и должен использоваться очень осторожно.

Запускаем **FastAPI** и **Celery**:
```python
uvicorn main:app
```
```
celery -A main.celery worker --loglevel=info -P gevent
```
 Отправляем сообщение:
 ![[Pasted image 20250425161535.png]]
 И в консоли **Celery** мы видим следующее сообщение: 
```
[2025-04-25 16:14:42,476: INFO/MainProcess] Task main.call_background_task[2a7f27e1-1e93-4520-b870-b912439bcd79] received
```
 Спустя 10 секунд мы увидим следующее:
 ```
[2025-04-25 16:14:52,485: WARNING/MainProcess] Background Task called!
[2025-04-25 16:14:52,485: WARNING/MainProcess] ya ebu sobak
[2025-04-25 16:14:52,497: INFO/MainProcess] Task main.call_background_task[2a7f27e1-1e93-4520-b870-b912439bcd79] succeeded in 10.020477400015807s: None
```
То есть наша задача успешно отработала
На данный момент наше приложение **FastAPI** и задача **Celery** хранятся в одном файле. Давайте рассмотрим следующий вариант, когда у нас будет основной файл `main.py`, в котором находится приложение **FastAPI** и второй файл `task.py` в котором будут храниться наши задачи.

Изменим код файла `main.py`:
```python
from fastapi import FastAPI
from celery import Celery
from task import call_background_task

app = FastAPI()

celery = Celery(
    __name__,
    broker='redis://127.0.0.1:6379/0',
    backend='redis://127.0.0.1:6379/0',
    broker_connection_retry_on_startup=True
)


@app.get("/")
async def hello_world(message: str):
    call_background_task.delay(message)
    return {'message': 'Hello World!'}
```

Создадим файл `task.py` следующего содержания:
```python
import time
from celery import shared_task

@shared_task()
def call_background_task(message):
    time.sleep(10)
    print(f"Background Task called!")
    print(message)
```
Вместо того, чтобы использовать `@celery.task`, мы используем декоратор `@shared_task`. Если мы этого не сделаем, это приведет к проблеме круговой зависимости. `main.py`потребуется `tasks.py` для определения функции `call_background_task`, а `task.py` потребуется экземпляр **Celery**, импортированный из `main.py`
#### [[Сценарии использования]]
#### [[Мониторинг задач]]