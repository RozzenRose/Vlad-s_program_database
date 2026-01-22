#кеш #redis 

**Redis** - очень мощный инструмента для управления кешем. То есть он позволяет сохранять данные в **ОЗУ** и выгружать их оттуда. **ОЗУ** - это очень быстрая память, в результате чего сохранение и выгрузка данных происходят мгновенно. В нем можно хранить данные, которые, часто нужны, из **Redis** данные можно получать гораздо быстрее, чем из **БД**. 

Создание объекта:
```python
import redis.asyncio as redis
from app.config import settings


class Redis:
    _redis = None

    @classmethod
    async def get_redis(cls):
        if cls._redis is None:
            cls._redis = redis.from_url(settings.redis_url, decode_responses=True)
        return cls._redis
```
Загрузить данные в **Redis**
```python
await redis.set(cache_key, json.dumps((data), ex=180) #ex - время жизни
```
Выгрузить данные из **Redis**
```python
cache = await redis.get(cache_key)
```
Удалить данные из **Redis**
```python
await redis.delete(cache_key)
```
# Кеширование
**Кеширование** - это когда мы при извлечении данных из **БД**, сохраняем их в **ОЗУ** на какой-то промежуток времени. В случае если мы получим идентичный запрос за период жизни кеша, данные не будут запрошены в **БД**, они просто извлекутся из кеша. 
### Пример
У нас есть функция извлечения данных из базы данных:
```python
async def get_all_incomes_from_db(db, user_id) -> list[Income]:
    query = select(Income).where(Income.owner_id==user_id)
    answer = await db.execute(query)
    data = answer.scalars().all()
    return data
```
Эта функция получает все записи о доходах одного пользователя. 

Добавим кеширование для этого запроса:
Для начала напишем класс для управления **Redis**:
```python
import redis.asyncio as redis
from app.config import settings


class Redis:
    _redis = None

    @classmethod
    async def get_redis(cls):
        if cls._redis is None:
            cls._redis = redis.from_url(settings.redis_url, decode_responses=True)
        return cls._redis
```
А затем добавим кеширование в функцию запроса
```python
async def get_all_incomes_from_db(db, user_id) -> list[Income]:
    redis = await Redis.get_redis()
    cache_key = f'{user_id}: incomes: all'
    cache = await redis.get(cache_key)
    if cache:
        data = json.loads(cache)
        return [Income.from_json(item) for item in data]
    query = select(Income).where(Income.owner_id==user_id)
    answer = await db.execute(query)
    data = answer.scalars().all()
    await redis.set(cache_key, json.dumps([item.to_dict() for item in data]), ex=180)
    return data
```
Эта функция при запуске всегда проверяет нет ли у нас нужных данных в **Redis**. Если данных в там нет, наша функция идет в **PostgreSQL** и получает данные там, но перед тем, как их вернуть сохраняет их в **Redis** на 3 минуты.