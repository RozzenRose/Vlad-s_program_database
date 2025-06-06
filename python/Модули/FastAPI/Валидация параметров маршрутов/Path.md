#FastAPI 

![[Pasted image 20250326161556.png]]
Рассмотрим класс `Path`, используемый для определения параметров пути в маршрутах. Он не только определяет, что параметр пути существует, но и накладывает правила валидации, улучшая работу с **API**. Класс `Path` также помогает дать параметрам маршрута больше контекста во время просмотра документации, автоматически предоставляемой `OpenAPI`, через `Swagger` и `ReDoc`, и действует как валидатор.

Класс `Path` работает только с параметрами пути

В частности, через конструктор `Path` можно установить следующие параметры для валидации значений:
- **Общие метаданные**:
	- `title`: описание параметра, отображаемое в документации.
	- `description`: более подробное описание параметра.
	- `example`: пример значения параметра.
	- `include_in_schema`: параметр позволяет исключить исключить операцию пути из сгенерированной схемы **OpenAPI**
-  **Специфичные правила валидации**:
	- `min_length`: минимальная длина строки для параметра.
	- `max_length`: максимальная длина строки для параметра.
	- `pattern`: устанавливает регулярное выражение, которому должно соответствовать значение параметра
	- `lt`: значение должно быть меньше указанного
	- `le`: значение должно быть меньше или равно указанному
	- `gt`: значение должно быть больше указанного
	- `ge`: значение должно быть больше или равно указанному

Рассмотрим некоторые возможности. Давайте переделаем код с использованием класса `Path`:
```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/user/{username}/{age}")
async def login(username: str = Path(min_length=3, max_length=15, description='Enter your username', example='Rozzem'),
                age: int = Path(ge=0, le=100, description="Enter your age")) -> dict:
    return {"user": username, "age": age}
```
Не трудно заметить, что параметр `username` получил дефолтное значение в виде экземпляра класса `Path` с некоторыми параметрами. А именно, у этого параметра появились ограничения по минимальной и максимальной длине строк, небольшое описание и пример корректного ввода.

Второй параметр `age` тоже получил в качестве дефолтного значения экземпляр класса `Path`, мы установили границы числа для поля `age` от `0` до `100` и добавили описание поля. Перейдем к просмотру документации  [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs):
![[Pasted image 20250326160225.png]]
Попробуем ради эксперимента передать данные, которые провалят валидацию:
![[Pasted image 20250326160417.png]]
Мы видим что параметр пути `name` длиной менее `3` символов, а параметр `age` это число более `100`. В результате валидация не была пройдена.

Также в случае, если мы перейдем по адресу [127.0.0.1:8000/user/Rozzen/29](http://127.0.0.1:8000/user/Rozzen/29), мы получим результат выполнения функции, ожидаемо.
Но в случае перехода по адресу [127.0.0.1:8000/user/RR/120](http://127.0.0.1:8000/user/RR/120) мы получим следующую ошибку валидации:
```JSON
{
  "detail": [
    {
      "type": "string_too_short",
      "loc": [
        "path",
        "username"
      ],
      "msg": "String should have at least 3 characters",
      "input": "RR",
      "ctx": {
        "min_length": 3
      }
    },
    {
      "type": "less_than_equal",
      "loc": [
        "path",
        "age"
      ],
      "msg": "Input should be less than or equal to 100",
      "input": "120",
      "ctx": {
        "le": 100
      }
    }
  ]
}
```

### Порядок параметров
Допустим, вы хотите  объявить `age` как обязательный параметр  запроса типа `int` к прошлому коду. Но нам по-прежнему нужно использовать `Path` для параметра пути `surname`
```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/user/{username}")
async def login(username: str = Path(min_length=3, max_length=15, description='Enter your username', example='Ilya'), age: int) -> dict:
    return {"user": username, "age": age}
```
Если вы поместите параметр запроса без значения по умолчанию после параметра пути, у которого нет значения по умолчанию, то **Python** укажет ошибку.
![[Pasted image 20250327114244.png]]
Также при запуске  сервера мы получим ошибку - `SyntaxError: parameter without a default follows parameter with a default`

Но вы можете изменить порядок параметров, чтобы параметр без значения по умолчанию (параметр запроса `age`) шел первым.
```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/user/{username}")
async def login(age: int,
                username: str = Path(min_length=3, max_length=15, description='Enter your username', example='Ilya')) -> dict:
    return {"user": username, "age": age}
```
Это не имеет значения для `FastAPI`. Он распознает параметры по их названиям, типам и значениям, ему не важен их порядок. Но данный код выглядит не совсем логично, поэтому мы можем использовать класс `Annotated`. И теперь мы больше не столкнемся с этой проблемой.
```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/user/{username}")
async def login(
        username: Annotated[
            str, Path(min_length=3, max_length=15, description='Enter your username',
                      example='Ilya')],
        age: int) -> dict:
    return {"user": username, "age": age}
```
И в заключении хотелось бы добавить, что `Path`- параметр всегда является обязательным, поскольку он составляет часть пути. Если вы установите для него значение по умолчанию, то это вызовет ошибку.