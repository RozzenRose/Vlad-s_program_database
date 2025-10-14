#aio_pika #RabbitMQ 

![[Pasted image 20251013140539.png]]
Для начала нам нужно построить инфраструктуру, для этого создаем в проекте следующую папку и файлы:
![[Pasted image 20251013135008.png]]
Все начинается с `connection.py`:
```python
import aio_pika
from aio_pika.abc import AbstractRobustConnection, AbstractChannel
from typing import Optional
from app.config import settings


class RabbitMQConnectionManager:
    _connection: Optional[AbstractRobustConnection] = None
    _channels: Optional[AbstractRobustChannel] = {}
    _url: str = settings.rabbitmq_url


    @classmethod
    async def get_connection(cls) -> AbstractRobustConnection:
        if cls._connection is None or cls._connection.is_closed:
            cls._connection = await aio_pika.connect_robust(cls._url)
        return cls._connection


    @classmethod
    async def get_channel(cls, name: str = "default") -> AbstractRobustChannel:
        if name not in cls._channels or cls._channels[name].is_closed:
            connection = await cls.get_connection()
            cls._channels[name] = await connection.channel()
        return cls._channels[name]


    @classmethod
    async def close_all(cls):
        """Закрывает все каналы и соединение"""
        for name, channel in cls._channels.items():
            if channel and not channel.is_closed:
                await channel.close()
        cls._channels.clear()

        if cls._connection and not cls._connection.is_closed:
            await cls._connection.close()

```
Если `from app.config import settings` еще не готов, или вообще не планируется, можно использовать дефолтный `url`: `'amqp://guest:guest@localhost:5672'`. 

Простой класс для создания каналов. Если запросить у этого класса канал, он проверить существует ли открытый канал, если его нет, он попытается создать новый канал из коннекта. Для этого он проверит есть ли открытый коннект, и в случае, если его нет, он создаст новый. Этот класс нужен чисто для инфраструктурной организации сущностей.

Далее разберем файл `rabbit_functions.py`:
```python
import aio_pika, asyncio, uuid, json
from app.rabbitmq import RabbitMQConnectionManager


async def consume_response(reply_queue, correlation_id: str, future: asyncio.Future):
    async def on_message(message: aio_pika.IncomingMessage) -> None:
	    if message.correlation_id == correlation_id and not future.done():
	        future.set_result(message.body)
	        await message.ack()
	    else:
	        await message.nack(requeue=True)

    consumer_tag = await reply_queue.consume(on_message)
    return consumer_tag


async def send_message(channel, data, queue, reply_queue, correlation_id):
    await channel.default_exchange.publish(
        aio_pika.Message(body=data, reply_to=reply_queue.name,
                         correlation_id=correlation_id),
        routing_key=queue.name)


async def rpc_request(future):
    correlation_id = str(uuid.uuid4())  # создаем уникальный id для сообщения
    channel = await RabbitMQConnectionManager.get_channel()
    queue = await channel.declare_queue('queue', durable=True)
    reply_queue = await channel.declare_queue('reply_queue', durable=True)
    consumer_tag = await consume_response(reply_queue, correlation_id, future)
    data = {'RabbitMQ': 'O, hi Mark!'}
    await send_message(channel, json.dumps(data).encode(), queue, reply_queue, correlation_id)
    return reply_queue, consumer_tag

```
Методы `consume_response()` и `send_message()` отвечают за получение сообщений и за их отправку. Метод `rpc_request()` нужен для организации **rpc**-запроса. То есть для запроса, на который мы будем ожидать ответ. 

Разберем этот код чуть позже, сначала посмотрим, как он вызывается, для этого придумаем вот такую функцию:
```python
async def rebbit_test():
    future = asyncio.get_running_loop().create_future()
    reply_queue, consumer_tag = await rpc_request(future)

    try:
        response = json.loads(await asyncio.wait_for(future, timeout=10))
        print(response)
    except asyncio.TimeoutError:
        print('somethink broken')
    finally:
        await reply_queue.cancel(consumer_tag)  # сворачиваем канал
```

### Разбор:
Теперь когда у нас есть весь код, попробуем разобрать принцип его работы:
Начнем с функции `rebbit_test()`
#### rebbit_test()
Первое, что мы делаем, создаем футуру:
```python
future = asyncio.get_running_loop().create_future()
```
`get_running_loop()` вернет объект текущего цикла событий, а `create_future()` создаст футуру. Это такой объект, который представляет из себя потенциальную переменную. Сразу после создания она пустая, но чуть позже мы ее заполним. Казалось бы почему бы не использовать обычную переменную? потому что обычную переменную  нельзя использовать в методе `wait_for()`, но об этом позже.

Далее вызывается метод `rpc_request(future)`, ему передается футура. Теперь разберемся, что происходит внутри этого метода:
##### rpc_request()
Изначально мы создаем уникальный `id` для нашего сообщения и получаем канал связи с `RebbitMQ`:
```python
correlation_id = str(uuid.uuid4())  # создаем уникальный id для сообщения
channel = await RabbitMQConnectionManager.get_channel()
```
Далее, мы создаем объекты очередей:
```python
queue = await channel.declare_queue('queue', durable=True)
reply_queue = await channel.declare_queue('reply_queue', durable=True)
```
Эти методы создают очереди внутри самого `RabbitMQ`, если их еще не существует. Мы будет отправлять запросы в очередь `queue` в другое приложение, а получать ответы на наше сообщение будем в очередь `reply_queue`. Но помимо этого они возвращают объекты очередей, которые понадобятся нам позже.

После этого мы сразу подписываемся на очередь, для ответов через наш самописный метод `consume_response`:
###### consume_response()
```python
consumer_tag = await consume_response(reply_queue, correlation_id, future)
```

Разберем `consume_response` подробнее:
```python
async def consume_response(reply_queue, correlation_id: str, future: asyncio.Future):
    async def on_message(message: aio_pika.IncomingMessage) -> None:
	    if message.correlation_id == correlation_id and not future.done():
	        future.set_result(message.body)
	        await message.ack()
	    else:
	        await message.nack(requeue=True)

    consumer_tag = await reply_queue.consume(on_message)
    return consumer_tag
```
Метод `consume()` подпишется на очередь, и вернет `consumer_tag` или объект подписки, который нам еще тоже понадобится. При этом `consume()` прогонит через `on_message()` все сообщения, которые окажутся в этой очереди. `on_message()` проверит, что сообщение в очереди ответов именно то, которое нам нужно. Мы подписываем все сообщения на отправку `correlation_id`, воркер с другой стороны подпишет ответ этим же `correlation_id`, так мы сможем понять, что это наш ответ, а не чей-то еще.

Метод `act()` отметит сообщение, как прочитанное, для `RabbitMQ` и `RabbitMQ` удалит это сообщение из очереди. А вот метод `nack()` отметит это сообщение, как неподходящее и `RabbitMQ` будет хранить его в очереди дальше, пока его не заберет кто-нибудь еще.

В случае, если это нужное нам сообщение, мы кладем его содержимое в нашу футуру:
```python
future.set_result(message.body)
```
##### rpc_request()
Вернемся к `rpc_request()`:
Создадим объект с данными на отправку:
```python
data = {'RabbitMQ': 'O, hi Mark!'}
```
И отправим их вокеру:
```python
await send_message(channel, json.dumps(data).encode(), queue, reply_queue, correlation_id)
```
###### send_message()
```python
async def send_message(channel, data, queue, reply_queue, correlation_id):
    await channel.default_exchange.publish(
        aio_pika.Message(body=data, reply_to=reply_queue.name,
                         correlation_id=correlation_id),
        routing_key=queue.name)
```
Мы вызываем метод `publish()` у ексчейнджера канала. И передаем ему в качестве аргумента сообщение. При сборке сообщения нужно заполнить аргументы, как в примере, можно не прописывать `correlation_id` если у очереди ответа будет только один слушатель. Так же можно не отмечать `reply_to`, если тебе не нужен ответ.
##### rpc_request()
```python
return reply_queue, consumer_tag
```
вернем эти объекты. Они еще пригодятся нам для обработки результата
#### rebbit_test()
```python
try:
	response = json.loads(await asyncio.wait_for(future, timeout=10))
	print(response)
except asyncio.TimeoutError:
	print('somethink broken')
finally:
	await reply_queue.cancel(consumer_tag)  # сворачиваем канал
```
Ну дальше все просто. 
```python
await asyncio.wait_for(future, timeout=10)
```
эта функция ожидания, она примет пустую футуру и будет смотреть на нее 10 секунд. Если за эти 10 секунд кто-то сетнет значение в футуру, функция просто вернет значение футуры и код продолжит выполнение. Футура сетится в методе `consume_response()`. Но если `wait_for()` не получит значения футуры за 10 секунд, она схлопнется и возбудит исключение `asyncio.TimeoutError`

В блоке `finally` мы аннулируем подписку на очередь ответов. Таким образом мы освобождаем ресурсы.

### Примечания
- Если у нас несколько воркеров, то нам понадобится несколько каналов