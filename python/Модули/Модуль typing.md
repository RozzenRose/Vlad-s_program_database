#python #анотации_типов 


Модуль `typing` содержит широкий функционал для работы с [[Анотации типов|аннотациями типов]]
```python
import typing
from typing import Any, Union, Optional
```
### Основные типы 
- `Any` - позволяет указать любой тип
- `Union` - указывает, что значение может быть одного из нескольких типов. Например, `Union[int, str]` может быть либо `int`, либо `str`.
- `Optional` - синтаксический #сахар для `Union[X, None]`, по сути обозначает, что аргумент может быть либо типа `X` или его вообще может не быть
```python
from typing import Any, Union, Optional

def process_data(data: Union[int, str]) -> Any:
    pass

def maybe_return_value(flag: bool) -> Optional[int]:
    return 42 if flag else 
```
### Коллекции
- `List`, `Tuple`, `Set`, `Dict`: типизированные версии стандартных коллекций
Например, `List[int]` — это список, содержащий только int.
```Python
from typing import List, Tuple, Dict

def process_list(data: List[int]) -> None:
    pass

def get_coordinates() -> Tuple[float, float]:
    return (45.0, 90.0)

def count_words(text: str) -> Dict[str, int]:
    return {"example": 1
```
### Callable
Тип для функций или других вызываемых объектов
```Python
from typing import Callable

def execute_function(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)
```
В конструкции `[int, int], int` внутри скобок указаны типы, которые в качестве аргументов принимает передаваемая функция, после этого указывается тип, которые функция возвращает
### Sequence и Iterable
Представляют последовательности и итерируемые объекты
```python
from typing import Sequence, Iterable

def sum_elements(data: Sequence[int]) -> int:
    return sum(data)

def print_elements(data: Iterable[str]) -> None:
    for item in data:
        print(item)
```
### Generic и TypeVar
`Generic` позволяет создавать класса, функции или интерфейсы с типизированными параметрами. Это полезно, когда логика работы класса или функции не зависит от конкретного типа данных, с которым она работает
```python
from typing import TypeVar, Generic, List

T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self):
        self.items: List[T] = []

    def push(self, item: T) -> None:
        self.items.append(item)

    def pop(self) -> T:
        return self.items.pop()

stack = Stack[int]()
stack.push(1)
stack.push(2)
print(stack.pop())  # 2
print(stack.pop())  
```
### NewType
Позволяет использовать в аннотациях экземпляры кастомных классов
```Python
from typing import NewType

UserId = NewType('UserId', int)

def get_user(user_id: UserId) -> None:
    
```
### Literal
Позволяет ограничить значения конкретными литералами
```Python
from typing import Literal

def draw_shape(shape: Literal['circle', 'square']) -> None:
    if shape == 'circle':
        print("Drawing a circle")
    else:
        print("Drawing a square"
```