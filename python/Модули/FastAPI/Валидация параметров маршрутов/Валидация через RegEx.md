#FastAPI 

![[Pasted image 20250326162633.png]]

Регулярные  выражения (**RegEx**) - это последовательность символов, которая определяет шаблон поиска.

Модуль `re`, включенный в стандартную библиотеку `python`, реализует функциональность регулярных выражений. Классы `Path()` и `Query()` в **FastAPI** позволяют определить параметр `pattern` для проверки, чтобы можно было проверить строковое значение параметра пути/запроса по указанному шаблону поиска.

Давайте добавим параметр `pattern` в класс `Query()`, чтобы ограничить значение `first_name` либо начинаться с `J`, либо заканчиваться на `s` (как например John, Tomas).
```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/user/{username}")
async def login(
        username: Annotated[
            str, Path(min_length=3, max_length=15, description='Enter your username',
                      example='Rozzen')],
        first_name: Annotated[
            str | None, Query(max_length=10, pattern="^J|s$")] = None) -> dict:
    return {"user": username, "Name": first_name}
```
Запустим приложение и проверим работу валидатора через регулярные выражения. Если мы вводим в поле `first_name` текст `Jhon`, то проверка выполняется успешно.
![[Pasted image 20250327163422.png]]
Но если мы  вводим имя, которое но подходит под наше регулярное выражение, то получаем ошибку:
![[Pasted image 20250327163516.png]]
