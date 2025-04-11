#FastAPI #status

![[e6b9e6e814b67b43767a6ce4facfb3fc.jpg]]
Этот класс содержит набор констант для стандартных **HTTP**-запросов
```python
from fastapi import FastAPI, status
```
Для использования мы просто создаем параметр `status_code` в декораторе и передаем в него нужный атрибут класса `status`. Пример:
```python
from fastapi import FastAPI, status

app = FastAPI()

@app.get("/", status_code=status.HTTP_200_OK)
def read_root():
    return {"message": "Hello, World!"}

@app.post("/items/", status_code=status.HTTP_201_CREATED)
def create_item(item: dict):
    return {"message": "Item created", "item": item}

@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_item(item_id: int):
    return {"message": f"Item {item_id} deleted"}
```
### 🔹 Основные атрибуты `status`
Все атрибуты представляют собой HTTP-статусы в виде чисел. Вот основные группы:
✅ **Успешные (2xx)**
```python
status.HTTP_200_OK               # 200 - Успешный запрос
status.HTTP_201_CREATED          # 201 - Успешное создание
status.HTTP_204_NO_CONTENT       # 204 - Нет содержимого
```
⚠️ **Клиентские ошибки (4xx)**
```python
status.HTTP_400_BAD_REQUEST       # 400 - Неверный запрос
status.HTTP_401_UNAUTHORIZED      # 401 - Неавторизованный
status.HTTP_403_FORBIDDEN         # 403 - Доступ запрещен
status.HTTP_404_NOT_FOUND         # 404 - Не найдено
status.HTTP_422_UNPROCESSABLE_ENTITY  # 422 - Ошибка валидации
```
❌ **Серверные ошибки (5xx)**
```python
status.HTTP_500_INTERNAL_SERVER_ERROR  # 500 - Ошибка сервера
status.HTTP_503_SERVICE_UNAVAILABLE    # 503 - Сервис недоступен
```