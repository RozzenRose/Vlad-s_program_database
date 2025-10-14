#aio_pika #RabbitMQ

![[Pasted image 20251013140547.png]]
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

Файл `rbmq_functions.py` будет  содержать функции для взаимодействия с **RabbitMQ**:
```python
import aio_pika, json
from aio_pika import Message
from logic.aggregator import Aggregator


async def handle_message(message: aio_pika.IncomingMessage, channel: aio_pika.Channel):
    async with message.process():
        body = message.body
        correlation_id = message.correlation_id
        reply_to = message.reply_to

        if not reply_to or not correlation_id:
            print("Пропущено сообщение без reply_to/correlation_id")
            return

        data = json.loads(body.decode())

        result = await calculator.calculate() # Выполнение нашей задачи

        await channel.default_exchange.publish( # Отправка ответа
            Message(
                body=json.dumps(result).encode('utf-8'),
                correlation_id=correlation_id
            ),
            routing_key=reply_to
        )

        print(f"Ответ отправлен в {reply_to} с correlation_id {correlation_id}")


async def consume_queue(queue_name: str, channel: aio_pika.Channel):
	'''Получение и обработка сообщений'''
    queue = await channel.declare_queue(queue_name, durable=True)
    async with queue.iterator() as queue_iter:
        async for message in queue_iter:
            await handle_message(message, channel)

```

Дальше напишем `main()`:
```python
import asyncio
from rabbitmq import consume_queue, RabbitMQConnectionManager


async def main():
    channel = await RabbitMQConnectionManager.get_channel()

    # Список очередей, на которые подписываемся
    queues = [
        "queue1",
        "queue2"
    ]

    # Запускаем слушателей параллельно
    await asyncio.gather(*(consume_queue(q, channel) for q in queues))


if __name__ == "__main__":
    asyncio.run(main())

```
`main()` очень простой, не вижу смысла его объяснять. Самое главное, что мы асинхронно подписываемся сразу на несколько очередей. Подписка происходит через функцию `consume_queue()`. 

