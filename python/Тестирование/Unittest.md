#тестирование #unittest

**Unittest** - наверное самая популярная библиотека для тестирования. По умолчанию встроена в Python.

### Формат кода
- тесты должны быть написаны в классе
- класс должен быть наследован от базового класса `unittest.TestCase`
- имена всех функций, являющихся тестами, должны начинаться с ключевого слова `test`
- внутри функций должны быть вызовы  операторов сравнения `assertX` - именно они будут проверять наши полученные значения на соответствие заявленным

Пример использования **unittest** для нашей задачи:
```python
import unittest

class SquareEqSolverTestCase(unittest.TestCase):
   def test_no_root(self):
       res = square_eq_solver(10, 0, 2)
       self.assertEqual(len(res), 0)

   def test_single_root(self):
       res = square_eq_solver(10, 0, 0)
       self.assertEqual(len(res), 1)
       self.assertEqual(res, [0])

   def test_multiple_root(self):
       res = square_eq_solver(2, 5, -3)
       self.assertEqual(len(res), 2)
       self.assertEqual(res, [0.5, -3])
```
`assertEqual()` просто сравнивает первый аргумент со вторым, если они равны, то проверка выполнена.

Запускается этот код следующей командой
`python.exe -m unittest example.py`
И в результате на экран будет выведено:
```
> python.exe -m unittest example.py
...
------------------------------------------------------------------
Ran 3 tests in 0.001s

OK
```
В случае, если в каком-нибудь из тестов будет обнаружена ошибка, **unittest** не замедлит о ней сообщить:
```
> python.exe -m unittest example.py
F..
==================================================================
FAIL: test_multiple_root (hello.SquareEqSolverTestCase)
------------------------------------------------------------------
Traceback (most recent call last):
  File "C:PyProjectstprogerexample.py", line 101, in test_multiple_root
    self.assertEqual(len(res), 3)
AssertionError: 2 != 3
------------------------------------------------------------------
Ran 3 tests in 0.001s

FAILED (failures=1)
```
### Unittest: аргументы “за”
- Является частью стандартной библиотеки языка Python: не нужно устанавливать ничего дополнительно;
- Гибкая структура и условия запуска тестов. Для каждого теста можно назначить теги, в соответствии с которыми будем запускаться либо одна, либо другая группа тестов;
- Быстрая генерация отчетов о проведенном тестировании, как в формате plaintext, так и в формате XML.
### Unittest: аргументы “против”
- Для проведения тестирования придётся написать достаточно большое количество кода (по сравнению с другими библиотеками);
- Из-за того, что разработчики вдохновлялись форматом библиотеки JUnit, названия основных функций написаны в стиле camelCase (например setUp и assertEqual);
- В языке python согласно рекомендациям [pep8](https://peps.python.org/pep-0008/) должен использоваться формат названий snake_case (например set_up и assert_equal).

### Мок функций
#### Мок атрибута класса
Атрибуты класса можно мокать с помощью `patch`. Важно понимать, что такие моки работают на уровне самого класса, а не конкретного объекта.
```python
from unittest.mock import patch

class MyClass:    
	class_attribute = 'original'
	
with patch('__main__.MyClass.class_attribute', new='mocked'):
	print(MyClass.class_attribute)  # mocked
print(MyClass.class_attribute)  # original
```
Почему так происходит? потому что **patch** временно изменяет значение только внутри контекста. Как только **with**-блок заканчивается, все возвращается на круги своя.
```Python
class OuterClass:    
	inner = MyClass()

with patch.object(OuterClass.inner, 'class_attribute', 'deep_mocked'):
	print(OuterClass.inner.class_attribute)  # deep_mocked
```
Это применимо, когда объект создается динамически, и нужно контролировать его поведение на разных уровнях вложенности.
#### Мок атрибута экземпляр
Тут немного сложнее. У экземпляра есть свой атрибут, и его можно подменять на ходу:
```Python
from unittest.mock import patch

class MyClass:    
	def __init__(self):        
		self.instance_attribute = 'original'

obj = MyClass()
with patch.object(obj, 'instance_attribute', 'mocked'):   
	print(obj.instance_attribute)  # mocked
print(obj.instance_attribute)  # original
```
Если хочешь подменить все будущие экземпляры класса, а не только один объект, можно сделать вот так:
```python
with patch.object(MyClass, 'instance_attribute', 'mocked', create=True):
	obj1 = MyClass()    
	obj2 = MyClass()    
	print(obj1.instance_attribute, obj2.instance_attribute)  # mocked mocked
```
Но если у объекта уже есть атрибут, `create=True` не нужен.
#### Как работает side_effect?
Как мы мокируем функции, чаще всего мы просто указываем `return_value`, чтобы она всегда возвращала одно и то же. Это удобно, но иногда недостаточно. Что если поведение функции зависит от переданных аргументов? Или она должна выбрасывать исключения в определенных ситуациях? Или вообще каждый раз возвращать разное значение?

 Вот тут и приходит на помощь `side_effect`

**side_effect как список значений**
Самый базовый вариант: даем моку список значений, и он будет возвращать их по порядку при каждом вызове.
```Python
from unittest.mock import Mock

mock = Mock(side_effect=[1, 2, 3, Exception("Больше нельзя!")])

print(mock())  # 1
print(mock())  # 2
print(mock())  # 3

try:    
	print(mock())  # Бросает исключение!
except Exception as e:    
	print(f"Ошибка: {e}")
```
Когда значения в списке заканчиваются, будет выброшено **StopIteration**. Это может быть полезно, когда мокируем итератор или **API**-запрос, который возвращает разные результаты при каждом вызове.

**side_effect как функция**
Если хотим, чтобы мок динамически реагировал на переданные аргументы - даём `side_effect` функцию.
```Python
from unittest.mock import mock_open, patch

mock = mock_open(read_data='file content')
with patch('builtins.open', mock):    
	assert read_file('dummy.txt') == 'file content'
```
Теперь мок ведет себя как функция. 

Еще пример - подмена **API**-функции, которая должна возвращать разные результаты для разных входных данных:
```python
def api_mock(endpoint):    
	responses = {        
		"/users": ["Alice", "Bob", "Charlie"],
        "/status": "OK",
        "/error": Exception("API недоступен"),
    }    
    if endpoint in responses:        
	    result = responses[endpoint]        
	    if isinstance(result, Exception):            
		    raise result        
		return result    
	return None
	
mock_api = Mock(side_effect=api_mock)

print(mock_api("/users"))   # ["Alice", "Bob", "Charlie"]
print(mock_api("/status"))  # "OK"

try:    
	print(mock_api("/error"))  # Выбросит Exception
except Exception as e:    
	print(f"Ошибка: {e}")
```

**side_effect для выбрасывания исключений**
Иногда мок **должен сломаться** при вызове, например, если он эмулирует сеть, которая может упасть.
```python
mock = Mock(side_effect=RuntimeError("Аварийное завершение!"))

try:   
	mock()  # бах!
except RuntimeError as e:    
	print(f"Поймано исключение: {e}")
```
Это применимо, когда тестируем код, который должен обрабатывать ошибки:
```python
def process_data(fetcher):    
	try:        
		data = fetcher()        
		return f"Данные получены: {data}"    
	except Exception as e:        
		return f"Ошибка: {e}"

mock = Mock(side_effect=ConnectionError("Нет сети"))
print(process_data(mock))  # Ошибка: Нет сети
```

**Смешанный side_effect**
Можно смешивать все вышеперечисленное:
```python
from unittest.mock import Mock

def crazy_mock(x):
	if x < 0:        
		raise ValueError("Отрицательные числа нельзя!")
	return x ** 2  # Возводим в квадрат
	
mock = Mock(side_effect=crazy_mock)

print(mock(2))   # 4
print(mock(5))   # 25

try:    
	print(mock(-1))  # Ошибка!
except ValueError as e:    
	print(f"Ошибка: {e}")
```
Здесь мок **работает как функция, но в определённых случаях выбрасывает исключение**.

**Как проверить порядок вызовов мока?**
В тестах бывает важно не просто проверить, **что методы были вызваны**, но и **в каком порядке**. Например, если мы тестируем логику оркестрации запросов в API‑клиенте, проверяем порядок вызова методов у объекта, или убеждаемся, что наш код **не перемешивает вызовы**.

В unittest.mock для этого есть несколько удобных инструментов:
- `mock_calls` — список всех вызовов моков в порядке выполнения
- `assert_has_calls()` — проверяет, что вызовы были, но не следит за их порядком
- `assert_called_once_with()` — проверяет конкретный вызов
- `call()` — удобный способ описать ожидаемую последовательность вызовов

### Функции обращения к БД
Вся фишка в том, что нам нужно заменить объект обращения к БД на специальный объект, который заранее должен знать, что нужно вернуть.

Допустим у нас есть функция, которая получает пользоваться по ID:
```python
def get_user_by_id(user_id, db):
    return db.get_user(user_id)
```
Где `db.gett_user` - это обращение к базе данных.

Тест без обращения к БД:
```python
from unittest.mock import MagicMock

def test_get_user_by_id():
    # Создаём мок базы данных
    mock_db = MagicMock()
    mock_db.get_user.return_value = {'id': 1, 'name': 'Alice'}

    # Вызываем тестируемую функцию
    result = get_user_by_id(1, mock_db)

    # Проверяем результат
    assert result == {'id': 1, 'name': 'Alice'}
    mock_db.get_user.assert_called_once_with(1)
```

Разберем на рабочем проекте:
```Python
from fastapi import APIRouter, Depends, status, HTTPException  
from sqlalchemy.ext.asyncio import AsyncSession  
from typing import Annotated  
from sqlalchemy import insert, select, update, func  
from app.models.reviws import Review  
from app.models.products import Product  
from app.sсhemas import CreateReview  
  
from app.routers.permission import get_current_user  
from app.backend.db_depends import get_db  
  
  
router = APIRouter(prefix='/reviews', tags=['review'])  
  
  
@router.get('/')  
async def all_reviews(db: Annotated[AsyncSession, Depends(get_db)]):  
    answer = await db.scalars(select(Review).where(Review.is_active == True))  
    return answer.all()
```
Попробуем написать тест для этого ендпоинта:
```python
import unittest
from unittest.mock import AsyncMock, MagicMock
from fastapi import FastAPI
from httpx import AsyncClient
from app.routers import reviews
from app.backend.db_depends import get_db  # оригинальная зависимость

class TestReviewRouter(unittest.IsolatedAsyncioTestCase):
    async def asyncSetUp(self):
        # Создаём тестовое FastAPI-приложение
        # self.app = FastAPI() Притащить то что уже есть и завернуть его в TestClient()

        # Подключаем роутер
        self.app.include_router(reviews.router)

        # Создаём мок для AsyncSession
        self.mock_db = AsyncMock()

        # Мокаем результат scalars().all()
        mock_scalars_result = MagicMock()
        mock_scalars_result.all.return_value = [
            {'id': 1, 'content': 'Review A'},
            {'id': 2, 'content': 'Review B'}
        ]
        self.mock_db.scalars.return_value = mock_scalars_result

        # Переопределяем зависимость get_db
        async def override_get_db():
            yield self.mock_db
        self.app.dependency_overrides[get_db] = override_get_db

        # Создаём асинхронного клиента
        self.client = AsyncClient(app=self.app, base_url="http://test")

    async def asyncTearDown(self):
        await self.client.aclose()

    async def test_all_reviews(self):
        response = await self.client.get("/reviews/")
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.json(), [
            {'id': 1, 'content': 'Review A'},
            {'id': 2, 'content': 'Review B'}
        ])

        self.mock_db.scalars.assert_called_once()
```