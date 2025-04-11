#FastAPI #Body

![[Pasted image 20250328140753.png]]
Класс **`Body`** из `fastapi` используется для явного описания параметров **тела запроса** (HTTP Request Body).

📌 **Зачем нужен `Body`?**
- **Позволяет добавлять метаинформацию** к параметрам тела запроса (описания, примеры, валидацию).
- **Используется с Pydantic-моделями** для обработки входных данных.
- **Упрощает документацию** в OpenAPI (Swagger).

Пример:
```python
from fastapi import FastAPI, status, Body

app = FastAPI()

messages_db = {0: "First post in FastAPI"}

@app.post("/message", status_code=status.HTTP_201_CREATED)  
async def create_message(message: str = Body()) -> str:  
    messages_db.update({list(messages_db.keys())[-1]+1: message})  
    print(messages_db)  
    return 'Message created!'
```
Запустим приложение. Мы видим что поля с параметрами запросов у нас теперь нет, и документация предлагает передать информацию внутри тела запроса. Попробуем добавить запись. После нажатия кнопки отправки запроса, мы видим, что наше сообщение находится внутри тела запроса:
![[Pasted image 20250328134655.png]]
А в словарь `messages_db` была добавлена запись: 
![[Pasted image 20250328135007.png]]
Здесь `Body()` указывает, что параметр `message` должен быть передан в теле запроса.