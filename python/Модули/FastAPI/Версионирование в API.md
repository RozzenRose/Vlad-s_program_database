#FastAPI 

Разберем как реализовать управление версиями для приложения **FastAPI** с независимыми ответами для каждой версии. Это важно, если вы хотите поддерживать различные версии вашего **API** с различной логикой, а также предоставлять документацию для каждой версии.

Когда дело доходит до обслуживания **API**, управление версиями является критически важным аспектом для обеспечения обратной совместимости и беспрепятственного внесения изменений. **FastAPI** предоставляет различные подходы к реализации управления управления версиями **API** в **FastAPI**.
### Управление версиями URL:
Одним из самых простых методов управления версиями **API** является включение номера версии в **URL**. Например:
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/v1/products")
async def get_products_v1():
    return {"message": "Products API Version 1"}


@app.get("/v2/products")
async def get_products_v2():
    return {"message": "Products API Version 2"}
```
Этот метод позволяет четко дифференцировать различные версии, но может загромождать **URI** и требовать обслуживания нескольких конечных точек.
### Управление версиями параметров запроса:
Другой подход заключается в добавлении версии в качестве параметра запроса:
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/products/")
async def get_products(version: int = 1):
    if version == 1:
        return {"message": "Products API Version 1"}
    elif version == 2:
        return {"message": "Products API Version 2"}
```
Этот метод сохраняет пути в чистоте и позволяет управлять версиями, но немного загромождая логику конечной точки.
### Версионирование на основе пути с использованием префикса URL:
Лучшее решение - использовать подприложения каждой версии вашего **API** на собственный объект приложения. Таким образом, вы можете иметь большой контроль и гибкость  над каждой версией, а также иметь независимую документацию для каждой версии. Например, вы можете иметь `/v1/docs` и `/v2/docs` в качестве разных конечных точек для разных версий вашей документации.
```python
from fastapi import FastAPI


app = FastAPI()
app_v1 = FastAPI()
app_v2 = FastAPI()


@app_v1.get("/products")
async def get_products_v1():
    return {"message": "Products API Version 1"}


@app_v2.get("/products")
async def get_products_v2():
    return {"message": "Products API Version 2"}

app.mount("/v1", app_v1)
app.mount("/v2", app_v2)
```
Теперь, если вы запустите этот код через **uvicorn**, и перейдете по адресу [http://localhost:8000/v1/docs](http://localhost:8000/v1/docs) или [http://localhost:8000/v2/docs](http://localhost:8000/v2/docs), вы увидите независимую документацию для каждой версии вашего **API**:
![[Pasted image 20250422115952.png]]В случае, если вы хотите добавить описания в подприложения, вы можете передать некоторые аргументы в экземпляр **FastAPI** таким образом:
```python
app_v1 = FastAPI(
title="My API v1",
description="The first version of my API",
)

app_v2 = FastAPI(
title="My API v2",
description="The second version of my API",
)
```
Таким образом, вы можете легко управлять версиями и документацией приложения