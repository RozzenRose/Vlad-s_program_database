#aio_pika #RabbitMQ

![[Pasted image 20251013141001.png]]
Создадим в проекте вот такую структуру:
![[Pasted image 20251014132938.png]]
`__init__.py` не нужен, его мы создавать не будем.

`connection.py`:
```python
import aio_pika
from aio_pika.abc import AbstractRobustConnection, AbstractRobustChannel
from typing import Optional
from config import settings


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

`rbmq_function.py`:
```python
import aio_pika, json
from logic.report_builder import get_report
from .to_aggregator import send_message


async def handle_message(message: aio_pika.IncomingMessage, channel: aio_pika.Channel):
    async with message.process():
        body = message.body
        correlation_id = message.correlation_id
        reply_to = message.reply_to

        if not reply_to or not correlation_id:
            print("Пропущено сообщение без reply_to/correlation_id")
            return

        data = json.loads(body.decode())
        result = await get_report(data)
        await send_message(channel=channel, data=result, queue=reply_to,
                           reply_queue=None, correlation_id=correlation_id)

        print(f"Ответ отправлен в {reply_to} с correlation_id {correlation_id}")


async def consume_queue(queue_name: str, channel: aio_pika.Channel):
    queue = await channel.declare_queue(queue_name, durable=True)
    async with queue.iterator() as queue_iter:
        async for message in queue_iter:
            await handle_message(message, channel)

```

`to_aggregation.py`:
```python
import asyncio, aio_pika, uuid, json
from rabbitmq.connection import RabbitMQConnectionManager


async def send_message(channel, data, queue, reply_queue, correlation_id):
    await channel.default_exchange.publish(
        aio_pika.Message(body=data, reply_to=reply_queue,
                         correlation_id=correlation_id),
        routing_key=queue)


async def consume_response(reply_queue, correlation_id: str, future: asyncio.Future):
    async def on_message(message: aio_pika.IncomingMessage) -> None:
        async with message.process():
            if message.correlation_id == correlation_id and not future.done():
                future.set_result(message.body)

    consumer_tag = await reply_queue.consume(on_message)
    return consumer_tag


async def send_to_aggregator(data):
    loop = asyncio.get_running_loop()
    future = loop.create_future()
    correlation_id = str(uuid.uuid4())
    channel = await RabbitMQConnectionManager.get_channel()
    queue = await channel.declare_queue('report_aggregation_queue', durable=True)
    reply_queue = await channel.declare_queue('reply_report_aggregation_queue', durable=True)
    consumer_tag = await consume_response(reply_queue, correlation_id, future)
    new_data = {'data': data, 'content': 'Report'}
    await send_message(channel, json.dumps(new_data).encode(), queue.name, reply_queue.name, correlation_id)

    try:
        response = json.loads(await asyncio.wait_for(future, timeout=10))
        return response
    except asyncio.TimeoutError:
        print('Timeout error')
    finally:
        await reply_queue.cancel(consumer_tag)

```

Ну и конечно `main()`:
```python
import asyncio
from rabbitmq.to_aggregator import RabbitMQConnectionManager
from rabbitmq.rbmq_functions import consume_queue


async def main():
    channel = await RabbitMQConnectionManager.get_channel()

    # Список очередей, на которые подписываемся
    queues = ["report_queue"]

    # Запускаем слушателей параллельно
    await asyncio.gather(*(consume_queue(q, channel) for q in queues))


if __name__ == "__main__":
    asyncio.run(main())

```