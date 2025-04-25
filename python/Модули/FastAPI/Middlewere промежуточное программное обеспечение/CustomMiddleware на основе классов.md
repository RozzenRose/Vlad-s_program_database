#FastAPI #CustomMiddleware

За основу возьмем следующее приложение:
```python
from fastapi import FastAPI


app = FastAPI()


@app.get("/hello")
async def greeter():
    return {"Hello": "World"}


@app.get("/goodbye")
async def farewell():
    return {"Goodbye": "World"}
```
**Middleware** обрабатывает запросы перед их отправкой в определенные операции пути, и  обрабатывает ответы перед их возвратом. Это делает его идеальным для общих операций, которые мы хотим выполнять по каждому запросу и ответу, таких как регистрация времени между запросом и его ответом.

Чтобы создать его, мы должны определить класс:
```python
import time

class TimingMiddleware:
    def __init__(self, app):
        self.app = app

    async def __call__(self, scope, receive, send):
        start_time = time.time()
        await self.app(scope, receive, send)
        duration = time.time() - start_time
        print(f"Request duration: {duration:.10f} seconds")
```
Важной частью этого класса является метод `__call__`, который реализует спецификацию приложения **ASGI**. Это асинхронная функция, которая принимает `scope`, `receive` и `send`. Эти параметры необходимы для запроса **FastAPI**. В этом простом **Middleware** мы просто передаем их в наше приложение **FastAPI** в `self.app`

Подключим этот класс в проекте **FastAPI**:
```python
from fastapi import FastAPI
import time


class TimingMiddleware:
    def __init__(self, app):
        self.app = app

    async def __call__(self, scope, receive, send):
        start_time = time.time()
        await self.app(scope, receive, send)
        duration = time.time() - start_time
        print(f"Request duration: {duration:.10f} seconds")


app = FastAPI()
app.add_middleware(TimingMiddleware)


@app.get("/hello")
async def greeter():
    return {"Hello": "World"}


@app.get("/goodbye")
async def farewell():
    return {"Goodbye": "World"}
```
Запустим сервер и проверим работу. Теперь при каждой отправке запросов, мы будем видеть время выполнения этих запросов.
```
INFO:     127.0.0.1:53251 - "GET /docs HTTP/1.1" 200 OK
Request duration: 0.0003800392 seconds
INFO:     127.0.0.1:53251 - "GET /openapi.json HTTP/1.1" 200 OK
Request duration: 0.0003292561 seconds
INFO:     127.0.0.1:53251 - "GET /hello HTTP/1.1" 200 OK
Request duration: 0.0006790161 seconds
INFO:     127.0.0.1:53251 - "GET /goodbye HTTP/1.1" 200 OK
Request duration: 0.0006358624 seconds
```

Теперь наше промежуточное программное обеспечение будет работать каждый раз, когда мы запрашиваем один из определенных маршрутов **API**. Мы должны иметь это в виду при расширении возможностей - если функция `__call__` нашего **Middleware**  содержит слишком много обработки, это замедлит все наше приложение.