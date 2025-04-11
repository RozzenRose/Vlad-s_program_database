#FastAPI 

Продолжим работать над нашим **CRUD**. Добавим модели. И посмотрим на их преимущества и особенности.

Первым делом добавим в наш код модель:
```python
from pydantic import BaseModel

class Message(BaseModel):
	id: int
	text: str
```
По аналогии, как и раньше, наши записи содержат поля **ID** и текста. Ранее в функции, отвечающий за **POST** запрос, мы принимали параметр `message: str = Body()`, и далее увеличивали значение **ID** и записывали в словарь данные. Но так как использовать вложенный словарь в качестве БД тут будет не совсем уместно, первым делом переделаем нашу БД в виде списка:
```python
messages_db = []
```
Перейдем непосредственно к функции `create_message()` и внесем следующие изменения:
```python
from fastapi import FastAPI, status, Body  
from pydantic import BaseModel

app = FastAPI()

messages_db = []

class Message(BaseModel):  
    id: int  
    text: str

@app.post("/message", status_code=status.HTTP_201_CREATED)
async def create_message(message: Message) -> str:
    if messages_db:
        message.id = max(messages_db, key=lambda m: m.id).id + 1
    else:
        message.id = 0
    messages_db.append(message)
    return "Message created!"
```
Теперь мы принимаем в переменную `message` модель `Message`, которая содержит поля: `messagae.id` и `message.text`. Мы также увеличиваем значение **ID** исходя из максимального значения **ID** записи, прибавляем `1` и записываем модель в нашу БД(список), выводя сообщение об успешной записи. Если БД(список) пуст, то `message.id` будет равно `0`.

Запустим приложение через команду `uvicorn crud:app --port 8000` и проверим работу. Зайдем в [документацию](http://127.0.0.1:8000/docs/), и перейдем в функцию, отвечающую за **POST** запросы.

И мы встречаем самое первое изменение, у нас теперь нет параметров запроса. То есть все параметры теперь всегда будут находиться внутри содержимого тела запроса. Второе изменение, это то, что теперь у нас есть пример содержимого тела запроса.
![[Pasted image 20250328171342.png]]
На данном этапе пока не будем ничего отправлять в запросе, перейдем дальше к изменениям функциям отвечающим за **GET** запросы.

В функции `get_all_messages` мы изменим вывод всех записей, а именно изменим вывод данных. А в функции `get_message` мы теперь получаем `message_id` как тип данных `int`, и далее с помощью индексов списка возвращаем запись.
```python
from fastapi import FastAPI, status, Body  
from pydantic import BaseModel  
  
app = FastAPI()  

messages_db = []  
  
class Message(BaseModel):  
    id: int  
    text: str  
  
@app.get("/")  
async def get_all_massage() -> dict:  
    return {'Messages': messages_db}  
  
@app.get("/message/{message_id}")  
async def get_message(message_id: int):  
    return messages_db[message_id]
```
Перейдем к тестированию GET и POST запросов нашего приложения. Перейдя в POST запросы, мы не будем менять содержимое полей, и просто несколько раз нажмем кнопку `Execute`
![[Pasted image 20250328171744.png]]

Перейдем к тестированию **GET** и **POST** запросов.

Несколько раз нажмем кнопку `Execute` с методом **POST**:
![[Pasted image 20250328174658.png]]

А теперь запросим все имеющиеся записи через функцию **Get All Message**:
![[Pasted image 20250328174806.png]]

Ну и попробуем вывести конкретное сообщение через метод **Get Message**:
![[Pasted image 20250328174903.png]]

При добавлении новой записи через метод `POST` заполнения поля `id` происходит динамически, но метод все равно просит передать туда какой-то число:
```rust
curl -X 'POST' \
  'http://127.0.0.1:8000/message' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "id": 0,
  "text": "string"
}'
```
Но мы можем вырезать поле `id` из запроса в документации. Для этого необходимо в параметры модели `Pydantic` добавить конфигурацию, которая устанавливает дефолтную схему для этой модели:
```Python
class Message(BaseModel):
    id: int | None = None
    text: str
    
    model_config = {
        "json_schema_extra": {
            "examples":
                [
                    {
                        "text": "Simple message",
                    }
                ]
        }
    }
```
Все параметры модели `Pydantic` можно посмотреть в  [официальной документации](https://docs.pydantic.dev/latest/api/config/): 

Запустим приложение, и попробуем отправить **POST** запрос. Мы увидим что теперь в разделе `Example Value` у нас находится следующая схема:
```http
{
  "text": "Simple message"
}
```
Отправим несколько запросов с `'text': 'Simple message'`
В процессе мы можем проверим, что в теле запроса не передается `ID`:
```rust
curl -X 'POST' \
  'http://127.0.0.1:8000/message' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "text": "Simple message"
}'
```
После этого выполним метод **GET**:
![[Pasted image 20250331121506.png]]Как мы видим, записи добавляются, `ID` в них присваиваются самостоятельно.

Но у нас появляются проблемы с методами `PUT` и `DELTE`. Начнем с работы над запросов`PUT`:
```python
@app.put("/message/{message_id}")  
async def update_message(message_id: int, message: str = Body()) -> str:  
    edit_message = messages_db[message_id]  
    edit_message.text = message  
    return 'Message {message_id} updated!'
```
Разберем код: мы все также получаем `message_id` как тип `int`, и `message` как текст в теле запроса. Но так как у нас в списке находится следующее содержание:
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
    }
  ]
}
```
Важно помнить, что в списке не хранятся словари, это объекты класса `Message`. Первым делом мы получаем нужный элемент, используя срезы списка `message_db[message_id]`.

Теперь к полям данного экземпляра класса мы можем обращаться по его ключам `.id` и `.text`, что мы и делаем, записывая в поле этой модели значение переменной `message`, полученной из тела запроса.

Теперь осталось немного переписать функцию удаления записей:
```python
@app.delete("/message/{message_id}")  
async def delete_message(message_id: int) -> str:  
    for i in range(len(messages_db)):  
        if messages_db[i].id == message_id:  
            current_id = messages_db[i].id  
            del messages_db[i]  
            return f'Message {current_id} deleted!'
    return 'Message not found'
```
Нам придется залезть в каждый объект класса `Message` по очереди просматривая их параметры `id`. До тех пор, пока мы не встретим объект параметр `id` которого не будет совпадать с номером id, который мы хотим удалить, после чего мы просто удалим найденную запись

Функция удаления всех сообщений вообще не поменяется:
```python
@app.delete("/")
async def kill_message_all() -> str: 
	messages_db.clear() 
	return "All messages deleted!"
```
