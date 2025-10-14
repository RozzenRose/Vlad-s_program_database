#RabbitMQ #очередь

![[Pasted image 20250925155204.png]]
Очередь задач, нужна для того, что иметь возможность отправить задачу из одного приложения в другое. Используется для создания распределенных систем. Если например у нас есть приложение, которое принимает задачу от пользователя через HTTP, а потом ему нужно провести какие-то серьезные расчеты с использованием значительных ресурсов RabbitMQ поможет нам оптимизировать такое приложение. Вообще передавать данные из одного приложения в другое можно тысячей разных способов, например другим HTTP запросом. Преимущество очередей по типу **RabbitMQ** в том, что сообщение с данными на обработку не теряются, если по какой-то причине сообщение не было доставлено.

Итак у нас есть **API**, которое принимает **HTTP** запросы, и есть эндпоинт, который строит графики по запросу и составляет из них PDF. Построение графиков и упаковка в PDF - трудоемкая задача относительно. Если напишем такой функционал в наше **API** приложение, один и тот же интерпретатор будет и принимать **HTTP** запросы и строит графики, многопоточность не поможет, потому что это **Python**, у него есть **GIL**-ограничение и все такое. Поэтому оптимальным решением будет отправить задачу построения графика в другой интерпретатор - то есть в другое приложение. При чем второй интерпретатор может крутится на той же машине в соседнем потоке, а может вообще работать на другой машине, нам не принципиально.

Приложение, которое создает задачу называется обычно **Producer**. 
Приложение, которое выполняет задачу - **Worker**
![[Pasted image 20250925130645.png]]

Для начала напишем класс, который поможет нам менеджерить коннекты и каналы:
```python
import aio_pika
from aio_pika.abc import AbstractRobustConnection, AbstractChannel
from typing import Optional


class RabbitMQConnectionManager:
    _connection: Optional[AbstractRobustConnection] = None
    _channel: Optional[AbstractChannel] = None
    _url: str = "amqp://guest:guest@localhost:5672/"


    @classmethod
    async def get_connection(cls) -> AbstractRobustConnection:
        if cls._connection is None or cls._connection.is_closed:
            cls._connection = await aio_pika.connect_robust(cls._url)
        return cls._connection


    @classmethod
    async def get_channel(cls) -> AbstractChannel:
        if cls._channel is None or cls._channel.is_closed:
            connection = await cls.get_connection()
            cls._channel = await connection.channel()
        return cls._channel


    @classmethod
    async def close_connection(cls):
        if cls._channel and not cls._channel.is_closed:
            await cls._channel.close()
        if cls._connection and not cls._connection.is_closed:
            await cls._connection.close()

```
**Connection (соединение)**
- Это **TCP-соединение** между вашим приложением и брокером RabbitMQ.

**Channel (канал)**
- Это **виртуальное соединение внутри Connection**.
- Лёгкая сущность: можно создавать тысячи каналов на одно TCP-соединение.
- Producer и Consumer обычно работают через каналы, а не напрямую через Connection.
- Каждый канал независим: у него свои очереди, подтверждения и т.д.
#### Producer
И так начнем писать `Producer`, я напишу только ендпоинт, писать все **API**  я не буду.
```python
async def get_rab_report():

    loop = asyncio.get_running_loop() # получаем объект цикла событий
    future = loop.create_future() # создаем из него футуру
    correlation_id = str(uuid.uuid4()) # создаем уникальную подпись для сообщения
    channel = await RabbitMQConnectionManager.get_channel() #получаем канал
    # создаем объкеты очередей, первая для отправки сообщений
    # вторая для приема ответов
    queue = await channel.declare_queue('report_queue', durable=False)
    reply_queue = await channel.declare_queue('plot_queue', durable=True)

	# функция для обработки ответов от worker
    async def on_message(message: aio_pika.IncomingMessage) -> None:
        async with message.process():
            if message.correlation_id == correlation_id:
                future.set_result(message.body)

	# подписываемся на очередь с ответами
    consumer_tag = await reply_queue.consume(RabbitMQConnectionManager.on_message)
	
	# отправляем задачу воркеру
    await channel.default_exchange.publish(
        aio_pika.Message(body=b'O! Hi, Mark!', reply_to=reply_queue.name,
                         correlation_id=correlation_id),
        routing_key=queue.name)

    try:
	    # ждем ответ от worker 10 секунд
        response = await asyncio.wait_for(future, timeout=10)
        print(" [x] Получен ответ")
    except asyncio.TimeoutError:
        print(" [!] Не дождались ответа")
    finally:
        await reply_queue.cancel(consumer_tag)
```
#### Worker
```python
import asyncio
import aio_pika, json
from aio_pika import Message
from report_builder import get_report


async def main():
    # Подключаемся к RabbitMQ
    connection = await aio_pika.connect_robust("amqp://guest:guest@localhost:5672/")
    channel = await connection.channel()

    # Подписываемся на очередь запросов
    queue = await channel.declare_queue("report_queue", durable=False)

    async with queue.iterator() as queue_iter:
        async for message in queue_iter:
            async with message.process():
                # Берём тело и метаданные
                body = message.body
                correlation_id = message.correlation_id
                reply_to = message.reply_to
                data = json.loads(body.decode())

                if not reply_to or not correlation_id:
                    print("Пропущено сообщение без reply_to/correlation_id")
                    continue

                # Обрабатываем сообщение
                result = await do_some(data)

                # Отправляем результат в очередь reply_to
                await channel.default_exchange.publish(
                    Message(
                        body=result,
                        correlation_id=correlation_id
                    ),
                    routing_key=reply_to
                )

                print(f"Ответ отправлен в {reply_to} с correlation_id {correlation_id}")


if __name__ == "__main__":
    asyncio.run(main())

```