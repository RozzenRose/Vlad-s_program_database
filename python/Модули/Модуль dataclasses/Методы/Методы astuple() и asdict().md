#модуль #dataclasses #Python #классы_данных


В модуле `dataclasses` доступны функции `astuple()` и `asdict()`, которые используются для преобразования экземпляров классов данных в кортежи и словари соответственно
```python
from dataclasses import dataclass, astuple, asdict

@dataclass
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

print(astuple(person))
print(asdict(person))
```
```
('Гвидо', 'ван Россум', 67)
{'name': 'Гвидо', 'surname': 'ван Россум', 'age': 67}
```
