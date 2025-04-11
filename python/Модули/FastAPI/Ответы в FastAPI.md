#FastAPI 

![[Pasted image 20250403135819.png]]
Ответы являются неотъемлемой частью жизненного цикла **API**. Ответы - это данные, полученный при взаимодействии с маршрутом **API** с помощью любого из стандартных методов **HTTP**. Ответ **API** обычно предоставляется в формате **JSON** или **XML**, но также может быть в формате документа. **FastAPI** отправляет **JSON**-ответ клиенту по умолчанию. Ответ состоит из заголовка и тела.

Заголовок ответа состоит из статуса запроса и дополнительной информации, необходимой для доставки тела ответа. Примером информации, содержащейся в заголовке ответа, является `Content-Type`, который сообщает клиенту возвращаемый тип содержимого.
```http
 content-length: 18 
 content-type: application/json 
 date: Fri,26 Jan 2024 06:18:33 GMT 
 server: uvicorn 
```
Тело ответа, с другой стороны, предоставляет собой данные, запрошенные клиентом с сервера. тело ответа определяется из переменной заголовка `Content-Type` наиболее часто используемой является `application/json`. 
### Коды состояния
Коды состояния - это уникальные короткие коды, выдаваемые сервером в ответ на запрос клиента. Коды состояния ответа сгруппированы в пять категорий, каждая из которых обозначает отдельный ответ:
- 1XX: Запрос получен.
- 2XX: Запрос выполнен успешно.
- 3XX: Запрос перенаправлен.
- 4XX: Ошибка клиента.
- 5XX: Ошибка сервера.

Стандартная практика для создания веб-приложений, независимо от платформы, заключается в возврате соответствующих кодов состояния для отдельных событий. Код состояния **400** не должен возвращаться в случае ошибки сервера. Точно также как код состояния **200** не должен возвращаться для неудачной операции запроса.
### Построение моделей ответа
Модели ответа могут быть построены на базе **Pydantic**, но служат для другой цели. В определении путей маршрута мы имеем следующий код:
```python
from fastapi import FastAPI, status, Body  
from pydantic import BaseModel  
  
app = FastAPI()  

messages_db = []  
  
class Message(BaseModel):  
    id: int | None = None  
    text: str  
  
    model_config= {  
        "json_schema_extra": {  
            "examples":  
                [  
                    {  
                        "text": "Simple message",  
                    }  
                ]  
        }  
    }  
  
@app.get("/")  
async def get_all_massage() -> dict:  
    return {'Messages': messages_db}  
  
  
@app.get("/message/{message_id}")  
async def get_message(message_id: int):  
    return messages_db[message_id]  
  
  
@app.post("/message", status_code=status.HTTP_201_CREATED)  
async def create_message(message: Message) -> str:  
    message.id = len(messages_db)  
    messages_db.append(message)  
    print(messages_db)  
    return "Message created!"  

  
@app.put("/message/{message_id}")  
async def update_message(message_id: int, message: str = Body()) -> str:  
    edit_message = messages_db[message_id]  
    edit_message.text = message  
    return 'Message {message_id} updated!'  
  
@app.delete("/message/{message_id}")  
async def delete_message(message_id: int) -> str:  
    for i in range(len(messages_db)):  
        if messages_db[i].id == message_id:  
            current_id = messages_db[i].id  
            del messages_db[i]  
            return f'Message {current_id} deleted!'    return 'Message not found'  
  
@app.delete("/")  
async def kill_message_all() -> str:  
    messages_db.clear()  
    print(messages_db)  
    return 'All messages was deleted!'
```
Метод `@app.get("/")` возвращает нам список из записей, содержащихся в списке `messages_db`. Например:
```json
{
  "Messages": [
    {
      "id": 0,
      "text": "Simple message"
    },
    {
      "id": 1,
      "text": "Simple message"
    },
    {
      "id": 2,
      "text": "Simple message"
    }
  ]
}
```
В данном случае мы возвращаем словарь, но мы можем изменить вывод возвращаемой информации и изменив аннотации типов, и получать уже возвращаемую информацию в виде объектов модели `Message`:
```python
@app.get("/")
async def get_all_messages() -> list[Message]:
    return messages_db
```
Данная функция **GET** запроса, возвращает записи, которые хранятся в виде моделей `Message`
```json
[
  {
    "id": 0,
    "text": "Simple message"
  },
  {
    "id": 1,
    "text": "Simple message"
  },
  {
    "id": 2,
    "text": "Simple message"
  }
]
```
Также мы можем добавить валидацию вывода через модели и у функции, которая возвращает одну запись:
```python
@app.get(path="/message/{message_id}")
async def get_message(message_id: int) -> Message:
    return messages_db[message_id]
```
И теперь при получении одной записи, мы будем получать в виде объeкта  модели **Message**.

**FastAPI** будет использовать проверку типов данных при их возврате. Если данные недействительны (например. у нас отсутствует поле), это означает, что код нашего приложения не работает и не возвращает то, что должен. Таким образом, мы можем быть уверены, что получаем данные в ожидаемой форме данных.
