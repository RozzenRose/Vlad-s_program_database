#модуль #dataclasses #Python #классы_данных #field

```python
import dataclasses
```
### Классы данных
Иногда реализуем классы, экземпляры которых ведут себя как контейнеры для хранения информации, и при реализации каждого такого класса нам приходится тратить значительное количество времени на написание шаблонного кода, определяя массивный инициализатор с большим количеством аргументов. Решением этой проблемы являются `классы данных`. Они призваны автоматизировать процесс определения классов, которые используются для хранения информации

##### Методы:
- [[Функция field()]] - гибкий способ определения атрибутов
- [[Метод __post_init__()]] - второй дандер для инициализации в классе
- [[Методы astuple() и asdict()]] - представление класса данных в виде объектов типа `tuple` или `dict`
#### Создание
Рассмотрим класс `Person`, описывающий человека:
```python
class Person:
    def __init__(self, name, surname, age):
        self.name = name                                  # имя
        self.surname = surname                            # фамилия
        self.age = age                                    # возраст
```
Экземпляр данного класса содержит имя, фамилию и возраст человека, которые доступны по соответствующим атрибутам
```python
class Person:
    def __init__(self, name, surname, age):
        self.name = name
        self.surname = surname
        self.age = age


person = Person('Гвидо', 'ван Россум', 67)

print(person.name)
print(person.surname)
print(person.age)
```
```
Гвидо
ван Россум
67
```

Определить приведенный вышел класс `Person` в виде класса данных мы можем следующим образом:
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int
```
То есть, чтобы определить класс данных, мы должны воспользоваться декоратором `@dataclass` из модуля `dataclasses` и в теле класса перечислить имена всех атрибутов, которыми будут обладать его экземпляры. Также через двоеточие необходимо указать тип значения каждого атрибута
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

print(person.name)
print(person.surname)
print(person.age)
```
```
Гвидо
ван Россум
67
```
 Аннотации типов в классах данных обязательно должны присутствовать, но не обязательно должны соблюдаться. если мы не хотим указывать конкретный тип, мы можем указать тип `Any` из модуля `typing`

Основным преимуществом классов данных является то, что они автоматически реализуют минимально необходимый базовый функционал, определяя методы `__init__()` `__repr__()` и `__eq__()`, которые позволяют создавать экземпляры классов, выводить их в форматированном виде и сравнивать между собой
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

print(person)
print(person == Person('Илон', 'Маск', 51))
print(person == Person('Гвидо', 'ван Россум', 67))
```
```
Person(name='Гвидо', surname='ван Россум', age=67)
False
True
```
Альтернативным способом определения классов данных является использование функции `meta_dataclass()` из модуля `dataclasses`. Данная функция принимает в качестве аргументов имя класса и итерируемый объект с именами атрибутов и возвращает соответствующий класс данных
```python
from dataclasses import make_dataclass

Person = make_dataclass('Person', ['name', 'surname', 'age'])

person = Person('Гвидо', 'ван Россум', 67)

print(person)
```
```
Person(name='Гвидо', surname='ван Россум', age=67)
```
Создание классов данных с помощью функции `make_dataclass()` очень похоже на создание именованных кортежей

Прочитать про именованные кортежи можно в рамках нашего курса для профессионалов по [ссылке](https://stepik.org/lesson/590034/step/1?unit=584966).

#### Неизменяемость
По умолчанию экземпляры классов данных являются изменяемыми, поэтому мы можем без проблем менять значения их атрибутов, а также добавлять им новые атрибуты
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

person.name = 'Илон'
person.city = 'Бельмонт'

print(person.name)
print(person.city)
```
```
Илон
Бельмонт
```
Для того чтобы экземпляр класса данных были неизменяемым, при его определении необходимо передать декоратору `@dataclass` в качестве параметра `frozen` значение `True`
```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

person.name = 'Илон'
```
```
dataclasses.FrozenInstanceError: cannot assign to field 'name'
```

#### Значения по умолчанию
При определении класса данных мы можем указывать атрибутам значения по умолчанию. Синтаксически это выглядит как присваивание переменной некоторого значения
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int = None                # возраст по умолчанию равен None


person1 = Person('Гвидо', 'ван Россум')
person2 = Person('Илон', 'Маск', 51)

print(person1)
print(person2)
```
```
Person(name='Гвидо', surname='ван Россум', age=None)
Person(name='Илон', surname='Маск', age=51)
```
Однако стоит помнить о том, что атрибуты без значений по умолчанию не могут стоять после атрибутов со значениями по умолчанию
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str = None
    surname: str = None
    age: int
```
```
TypeError: non-default argument 'age' follows default argument
```

В классах данных изменяемые объекты нельзя указывать в качестве значений по умолчанию
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int
    friends: list = []                                    # список друзей
```
```
ValueError: mutable default <class 'list'> for field friends is not allowed: use default_factory
```
Обусловлено это тем, что значения по умолчанию создаются единожды при определении класса. И если бы мы могли указывать изменяемые объекты в качестве значений по умолчанию, то все экземпляры класса данных использовали бы один и тот же объект, и изменение его в одном экземпляре приводило бы к его изменению во всех экземпляра
(На самом деле можно, через [[Функция field()]])

#### Сравнение 
По умолчанию экземпляры класса данных можно сравнивать между собой лишь на равенство
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int


person1 = Person('Гвидо', 'ван Россум', 67)
person2 = Person('Илон', 'Маск', 51)

print(person1 > person2)
```
```
TypeError: '>' not supported between instances of 'Person' and 'Person'
```
Для того чтобы экземпляры класса данных можно было сравнивать на больше/меньше, при его определении необходимо передать декоратору `@dataclass` в качестве параметра `order` значение `True`
```python
from dataclasses import dataclass

@dataclass(order=True)
class Person:
    name: str
    surname: str
    age: int


person1 = Person('Гвидо', 'ван Россум', 67)
person2 = Person('Илон', 'Маск', 51)

print(person1 > person2)
```
```
False
```
Сравнение в таком случае подразумевает последовательное сравнение значений всех атрибутов, то есть сперва сравниваются значения атрибутов `name`, если они равны - сравниваются значения атрибутов `surname`, и так далее до первого несовпадения. В примере выше `person2` больше `person1`, так как `person2.name` больше `person1.name`. Таким образом, располагая атрибуты в теле класса в определенном порядке, мы можем контролировать их приоритет при сравнении

Также мы можем использовать функцию `field()`, чтобы исключить атрибуты, которые не хотим учитывать при сравнении
```python
from dataclasses import dataclass, field

@dataclass(order=True)
class Person:
    name: str = field(compare=False)                # атрибут не учитывается при сравнении
    surname: str = field(compare=False)             # атрибут не учитывается при сравнении
    age: int


person1 = Person('Гвидо', 'ван Россум', 67)
person2 = Person('Илон', 'Маск', 51)

print(person1 > person2)
```
```
True
```

#### Дополнительные методы 
Классы данных с точки зрения `Python` являются обычными классами, поэтому помимо атрибутов и реализованных по умолчанию магических методов `__init__()`, `__repr__()` и `__eq__()` они могут иметь произвольный набор дополнительных методов
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int
    
    def fullname(self):     # метод экземпляра, возвращающий полное имя
        return self.name + ' ' + self.surname


person = Person('Гвидо', 'ван Россум', 67)

print(person.fullname())
```
```
Гвидо ван Россум
```
Уже реализованные методы `__init__()`, `__repr__()` и `__eq__()` также могут быть переопределены, однако их рекомендуется оставлять неизменными
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    surname: str
    age: int

    def __repr__(self):                      # переопределенный метод __repr__()
        return self.name + ' ' + self.surname


person = Person('Гвидо', 'ван Россум', 67)

print(person)
```
```
Гвидо ван Россум
```

#### Примечания
##### Примечание 1
Декоратор `@dataclass` имеет параметры `init`, `repr` и `eq`, отвечающие за автоматическую реализацию одноименных магических методов. По умолчанию данные параметры имеют значение `True`
```python
from dataclasses import dataclass

@dataclass(repr=False)                # не реализуем магический метод __repr__()
class Person:
    name: str
    surname: str
    age: int


person = Person('Гвидо', 'ван Россум', 67)

print(person)
```
```
<__main__.Person object at 0x0000015692B574C0>
```

##### Примечание 2
Подробнее о классах данных можно почитать на английском языке по [ссылке](https://realpython.com/python-data-classes/) и на русском языке по [ссылке](https://habr.com/ru/post/415829/) и [ссылке](https://habr.com/ru/company/otus/blog/650257/).

##### Примечание 3
У этого модуля есть более функциональный аналог - модуль `attrs`. Документация [тут](https://attrs.org/en/stable/)