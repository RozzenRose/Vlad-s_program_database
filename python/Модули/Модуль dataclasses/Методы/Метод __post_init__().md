#модуль #dataclasses #Python #классы_данных #post_init 

```python
from dataclasses import dataclass
```

В классах данных есть возможность определить второй инициализатор в виде метода `__post_init__()`, который, если определен, автоматически вызывается после метода `__init__()`. Он может быть удобен в случаях, когда мы хотим добавить экземпляру класса атрибут, значение которого зависит от значений других атрибутов
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int
    fullname: str = None
    
    def __post_init__(self):
        self.fullname = self.name + ' ' + self.surname    # полное имя на основе имени и фамилии


person = Person('Гвидо', 'ван Россум', 67)

print(person.fullname)
```
```
Гвидо ван Россум
```
Если мы хотим, чтобы значение атрибута, вычисляемого на основе других, нельзя было указывать при создании экземпляра класса, мы можем воспользоваться функцией `field()`
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int
    fullname: str = field(init=False)
    
    def __post_init__(self):
        self.fullname = self.name + ' ' + self.surname


person = Person('Гвидо', 'ван Россум', 67)

print(person.fullname)
```
```
Гвидо ван Россум
```

```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int
    fullname: str = field(init=False)
    
    def __post_init__(self):
        self.fullname = self.name + ' ' + self.surname


person = Person('Гвидо', 'ван Россум', 67, 'Гвидо ван Россум')
```
```
TypeError: Person.__init__() takes 4 positional arguments but 5 were given
```