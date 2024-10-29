#модуль #dataclasses #Python #классы_данных #field 


```python
from dataclasses import dataclass, field
```

Метод `field()` используется для создания специальных параметров для полей класса данных. Он позволяет задавать параметры, такие как значения по умолчанию, функции для создания значений по умолчанию, и дополнительные метаданные

Метод `field()` дает возможность использовать изменяемые объекты в качестве значений по умолчанию все же есть. Для этого необходимо воспользоваться функцией `field()` из модуля `dataclasses`, передав ей в качестве параметра `default_factory` функцию, возвращаемое значение которой и является желаемым значением по умолчанию
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int
    friends: list = field(default_factory=list) # функция list(), возвращающая пустой список

person = Person('Гвидо', 'ван Россум', 67)

print(person.friends)
```
```
[]
```
Так как при вычислении значения по умолчанию атрибута `friends` вызывается функция `list()`, все экземпляры класса `Person` имеют в качестве этого атрибута созданный конкретно для них отдельный пустой список
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int
    friends: list = field(default_factory=list)


person1 = Person('Гвидо', 'ван Россум', 67)
person2 = Person('Илон', 'Маск', 51)

person1.friends.append(person2)

print(person1.friends)
print(person2.friends)
```
```
[Person(name='Илон', surname='Маск', age=51, friends=[])]
[]
```
Функция `field()` также может использоваться для указания простых значений по умолчанию. Для этого необходимо передать ей в качестве параметра `default` желаемое значение по умолчанию
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int = field(default=None)


person1 = Person('Гвидо', 'ван Россум')
person2 = Person('Илон', 'Маск', 51)

print(person1)
print(person2)
```
```
Person(name='Гвидо', surname='ван Россум', age=None)
Person(name='Илон', surname='Маск', age=51)
```

Помимо установки значений по умолчанию функция `field()` предлагает еще несколько возможностей

1.  Параметр `repr` определяет, будет ли атрибут отображаться в строковом представлении экземпляра класса. По умолчанию данный параметр имеет значение `True`
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int = field(repr=False)


person = Person('Гвидо', 'ван Россум', 67)

print(person)
print(person.age)
```
```
Person(name='Гвидо', surname='ван Россум')
67
```

2. Параметр `compare` определяет, будет ли атрибут учитываться при сравнении экземпляров класса. По умолчанию данный параметр имеет значение `True`
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int = field(compare=False)


person1 = Person('Гвидо', 'ван Россум', 67)
person2 = Person('Гвидо', 'ван Россум', 68)

print(person1 == person2)
```
```
True
```

3. Параметр `init` определяет, будет ли атрибут участвовать при инициализации экземпляра класса. По умолчанию данный параметр имеет значение `True`
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int = field(init=False)


person = Person('Гвидо', 'ван Россум', 67)
```
```
TypeError: Person.__init__() takes 3 positional arguments but 4 were given
```
То есть если атрибут не участвует при инициализации экземпляра класса, то его значение не должно передаваться при создании этого экземпляра, так как инициализатор его не ждет. Также из этого следует, что экземпляр класса этим атрибутом обладать не будет
```python
from dataclasses import dataclass, field

@dataclass
class Person:
    name: str
    surname: str
    age: int = field(init=False)


person = Person('Гвидо', 'ван Россум')

print(hasattr(person, 'age'))
```
```
False
```
Указание в качестве параметра `init` значения `False` в таком виде достаточно бесполезно, ведь атрибут определяется в теле класса, но никак не используется. Однако это может быть удобно при использовании метода [[Метод __post_init__()]], о котором будет сказано позже.

4. Параметр `hash`определяет, будет ли атрибут учитываться при вычислении хеш-значения экземпляра класса. По умолчанию данный параметр имеет значение `True`