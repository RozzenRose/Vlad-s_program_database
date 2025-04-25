#FastAPI #CastomMiddleware

При создании промежуточного программного обеспечения на основе функций мы можем использовать декоратор `@app.middleware("http")`, поверх функции, чтобы указать, что функция будет действовать как промежуточное программное обеспечение. Это говорит **FastAPI** зарегистрировать функцию как компонент **Middleware**.

Возьмем за основу следующее приложение:
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
И напишем наш кастомную функцию **Middleware**, которая также будет считать время выполнения запросов:
```python
from fastapi import FastAPI, Request
import time

app = FastAPI()


@app.middleware("http")
async def modify_request_response_middleware(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    duration = time.time() - start_time
    print(f"Request duration: {duration:.10f} seconds")
    return response


@app.get("/hello")
async def greeter():
    return {"Hello": "World"}


@app.get("/goodbye")
async def farewell():
    return {"Goodbye": "World"}
```
Функция промежуточного программного обеспечения получает два параметра:
- **request**: Содержит всю информацию и данные, связанные с входящим запросом.
- **call_next**: Вызывая функцию `call_next`, промежуточное программное обеспечение гарантирует, что запрос переходит к соответствующей операции пути и ждет результирующего ответа.

Запустим сервер и проверим работу. Теперь при каждой отправке запросов, мы будем видеть время выполнения этих запросов.
```vbnet
INFO:     127.0.0.1:49289 - "GET /docs HTTP/1.1" 200 OK
Request duration: 0.0003540516 seconds
INFO:     127.0.0.1:49289 - "GET /openapi.json HTTP/1.1" 200 OK
Request duration: 0.0006909370 seconds
INFO:     127.0.0.1:49289 - "GET /hello HTTP/1.1" 200 OK
Request duration: 0.0006380081 seconds
INFO:     127.0.0.1:49289 - "GET /goodbye HTTP/1.1" 200 OK
```

Наше промежуточное программное обеспечение будет также, как и на классах, работать каждый раз, когда мы запрашиваем один из определенных маршрутов API.