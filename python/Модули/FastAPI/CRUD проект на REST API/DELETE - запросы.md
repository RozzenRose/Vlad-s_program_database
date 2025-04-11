#FastAPI #delete

![[Pasted image 20250328161944.png]]
Запрос `DELETE` - это метод, используемый в протоколе передачи гипертекста **HTTP** для запроса на удаление или удаление ресурса на сервере. Когда клиент отправляет запрос `DELETE` на сервер, он просит удалить указанный ресурс, такой как файл или запись базы данных.

Метод является идемпотентным, что означает, что выполнение нескольких одинаковых запросов должно иметь тот же эффект, что и выполнение одного запроса. Однако важно отметить, что фактическое удаление ресурса зависит от реализации и политики сервера. После поучения запроса `DELETE`, сервер обрабатывает его и удаляет указанный ресурс, если он существует, возвращая код состояния, указывающий на успех или неудачу операции.

В скелете нашего **API** уже есть функции, которые принимают `DELETE` запросы. Функция `delete_message` принимает `message_id` из параметра пути. А функция `kill_message_all` ничего не принимает.

Оба метода мы объявляли с помощью декоратора `@app.delete`. И напишем код удаления записей:
```python
from fastapi import FastAPI, status, Body  
  
app = FastAPI()  
  
messages_db = {0: "First post in FastAPI",
			   1: 'Second',
			   2: 'Third'}

@app.delete("/message/{message_id}")  
async def delete_message(message_id: int) -> str:  
    del messages_db[message_id]  
    print(messages_db)  
    return f'Message {message_id} deleted!'  
@app.delete("/")  
async def kill_message_all() -> str:  
    messages_db.clear()  
    print(messages_db)  
    return 'All messages was deleted!'
```
Параметр `status_code` мы заполнять не будем, так как при успешном выполнении метод `Delete` возвращает код ответа `200`.

Запустим приложение `uvicorn crud:app --port 8000`.
Зайдем в  [документацию](http://127.0.0.1:8000/docs/), попробуем отправить `DELTE` запрос через функцию `message_delete`:
![[Pasted image 20250328153559.png]]
Видим запись в консоли:
![[Pasted image 20250328153623.png]]

Испытаем функцию `kill_message_all()`:
![[Pasted image 20250328153744.png]]
Консоль:
![[Pasted image 20250328153804.png]]

В результате словарь оказывается пуст.