 #FastAPI 

![[Pasted image 20250325192427.png]]

Для работы с параметрами строки запроса **FastAPI** предоставляет класс `Query` из пакета `fastapi`, который используется для определения параметров запроса. Параметры запроса передаются в **URL**-адресе после знака вопроса (`?`), например `/items?item_id=123`

**Общие метаданные**:
- `title`: описание параметра, отображаемое в документации.
- `description`: более подробное описание параметра.
- `examples`: пример значения параметра
- `imclude_in_schema`: параметр позволяет исключить операцию пути из сгенерированной схемы **OpenAPI**
**Специфичные правила валидации**:
- `min_length`: минимальная длина строки для параметра.
- `max_length`: максимальная длина строки для параметра.
- `pattern`: устанавливает  регулярное выражение, которому должно соответствовать значение параметра
- `lt`: значение должно быть меньше указанного
- `le`: значение должно быть меньше или равно указанному
- `gt`: значение должно быть больше указанного
- `ge`: значение должно быть больше или равно указанному

Теперь, когда мы рассмотрели `Annotated` в [[Path]], будем использовать ее, и добавим `Querry` со значением параметра `max_length` равным `10`:
```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/user/{username}")
async def login(
        username: Annotated[
            str, Path(min_length=3, max_length=15, description='Enter your username',
                      example='permin0ff')],
        first_name: Annotated[str | None, Query(max_length=10)] = None) -> dict:
    return {"user": username, "Name": first_name}
```
Обратите внимание, что значение по умолчанию все еще `None`, так что параметр запроса будет необязательным. Давайте запустим приложение и попробуем передать запроса, например такой [http://127.0.0.1:8000/user/permin0ff?first_name=Rozzen](http://127.0.0.1:8000/user/permin0ff?first_name=Rozzen)
![[Pasted image 20250327120614.png]]
Мы можем сделать параметры не обязательными, указав:
```python
first_name: Annotated[str | None, Query()] = None)
```
Когда `Querty` используется внутри `Annotated`, вы не можете использовать параметр `default` у `Query`. Вместо этого, используйте обычное указание значения по умолчанию для параметра функции.
```python
first_name: Annotated[str | None, Query()] = "Tomas")
```
В случае, чтобы сделать параметр `Query` обязательным, вы можете просто не указывать значение по умолчанию, или установить параметр `default` равным многоточию `...`:
```Python
first_name: Annotated[str, Query(max_length=10)] = ...)
```
Если по каким-то причинам мы хотим использовать `Query` без `Annotated`, то следует использовать следующий синтаксис:
```python
first_name: str | None = Query(default=None, max_length=10))
```
Также с помощью параметра `include_in_schema` мы можем исключить данный параметр из документации.

С помощью класса `Query` можно запрашивать списки через строку запроса. Обычно списки значений появляются, когда в строке запросе один и тот же параметр повторяется с разными значениями. Это можно увидеть на примере запроса по определенному адресу:
[http://127.0.0.1:8000/user?people=tom&people=Sam&people=Bob](http://127.0.0.1:8000/user?people=tom&people=Sam&people=Bob)

Здесь параметру `people` передаются три разных значения, которые мы получим в виде списка:
```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/user")
async def search(people: Annotated[list[str], Query()]) -> dict:
    return {"user": people}
```
![[Pasted image 20250327160641.png]]
Документация **API** будет обновлена соответствующим образом, где будет разрешено множество значений:
![[Pasted image 20250327161453.png]]