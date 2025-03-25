#FastAPI #Redoc

![[Pasted image 20250325180344.png]]
**Redoc** - это еще один инструмент с открытым исходным кодом от  [Redocly](https://redocly.com/redoc/), который генерирует документацию API из определений **OpenAPI**. Он имеет панель поиска, навигационное меню, документацию и примеры запросов и ответов. 

Перейдем по адресу  [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc), чтобы отобразить  документацию **Redoc**
![[Pasted image 20250325141047.png]]В данном интерфейсе мы также можем отслеживать все возможности нашего **API**

Давайте рассмотрим, как мы можем удалить генерацию документации при инициализации приложения. Например. чтобы отключить `Swagger UI`, можем установить `docs_uel=None`, а чтобы отключить `Redoc`, установить `redoc_url=None`
```python
from fastapi import FastAPI

app = FastAPI(
    docs_url=None,  # Disable docs (Swagger UI)
    redoc_url=None,  # Disable redoc
)

@app.get("/")
async def welcome() -> dict:
    return {"message": "Hello World"}
```