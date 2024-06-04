#collections #Python #именнованныe_кортежи #namedtuple #объект #_fields #_fields_default #_make #_replace #_asdict


#### Функция `namedtuple()`
Эта функция выступает в роли фабричной функции, порождающей новые типы данных.
```python
namedtuple(typename, field_names, *, rename=False, defaults=None, module=None)
```
`typename` - имя создаваемого типа
`field_name` - названия полей, в качестве этого параметра можно использовать:
- список
- словарь
- кортеж
- строка
- множество
- итератор (та хрень, которую возвращают `map()` или `filter()`)
`rename` - параметр позволяющий переименовать данные для `field_name` в случае, если они содержат недопустимые значения
`dafault` - значение переменных по умолчанию
`module` - позволяет заменить имя модуля
##### Примеры field_names
`field_names` - является списком:
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])    # в качестве второго параметра передаем список
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

`field_names` - является кортежем:
```python
from collections import namedtuple

Point = namedtuple('Point', ('x', 'y'))    # в качестве второго параметра передаем кортеж
point =  Point(2, 4)
print(point)                               # выводит Point(x=2, y=4)
```

`field_names` - является словарем:
В этом случае для полей именованного кортежа используются ключи словаря, поэтому в качестве значений можно указать, все что угодно.
```python
from collections import namedtuple

Point = namedtuple('Point', {'x': 0, 'y': 69})    # в качестве второго параметра передаем словарь
point =  Point(2, 4)
print(point)                                      # выводит Point(x=2, y=4)
```

`field_names` - является строкой:
При создании именованного кортежа с помощью строки мы указываем поля либо через символ пробела, либо разделяя их символом `,`.
```python
from collections import namedtuple

Point = namedtuple('Point', 'x y')    # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                          # выводит Point(x=2, y=4)
```
либо:
```python
from collections import namedtuple

Point = namedtuple('Point', 'x,y')     # в качестве второго параметра передаем строку
point =  Point(2, 4)
print(point)                           # выводит Point(x=2, y=4)
```

`field_names` является множеством:
Так можно делать, но зачем? `set` это неупорядоченный набор данных. 
```python
from collections import namedtuple

Point = namedtuple('Point', {'x', 'y'})    # в качестве второго параметра передаем множество
point =  Point(2, 4)
print(point)
```
может быть как:
```
Point(x=2, y=4)
```
так и:
```
Point(y=2, x=4)
```

##### Примеры rename
Допустим, мы используем данные из CSV-файла и превращаем каждую строку в именованный кортеж. Структура файла имеет вид:
```
naem,surname,age,class
Timur,Guev,28,11
Ruslan,Chaniev,22,9
...
```
Названия полей мы берем из заголовка CSV-файла:
```python
from collections import namedtuple
headers = ('name', 'surname', 'age', 'class')
Student = namedtuple('Student', headers)
```
Поскольку одно поле имеет название `class` мы поучим ошибку. Для решения данной проблемы можно использовать параметр `rename` со значением `True`
```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class')
Student = namedtuple('Student', headers, rename=True)
stud = Student('Роман', 'Белых', 26, 10)

print(stud)
```
```
Student(name='Роман', surname='Белых', age=26, _3=10)
```
Python автоматически переименовал поле `class` в `_3`

```python
from collections import namedtuple

headers = ('name', 'surname', 'age', 'class', 'with', 'color', 'name', 'class', 'if')

Student = namedtuple('Student', headers, rename=True)

stud = Student('Тимур', 'Гуев', 28, 11, 'sister', 'green', 'Tim', '11A', 'else')
print(stud)
```
```
Student(name='Тимур', surname='Гуев', age=28, _3=11, _4='sister', color='green', _6='Tim', _7='11A', _8='else')
```
##### Паримеры defaults
Параметр используется для того, чтобы установить значения по умолчания для полей именованного кортежа.
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0, 0))
point1 = Point()      # используем значения по умолчанию
point2 = Point(1, 9)

print(point1)
print(point2)
```
```
Point(x=0, y=0)
Point(x=1, y=9)
```
Можно указать значение только для некоторых полей, при этом `defaut` присваивает значения по умолчанию с конца:
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], defaults=(0,))
point =  Point(7)      # используем значения по умолчанию для y
print(point)
```
```
Point(x=7, y=0)
```
##### Паримеры module
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point = Point(1, 2)
print(type(point))
```
```
<class '__main__.Point'>
```
Если мы укажем допустимое имя модуля для этого аргумента, тогда атрибуту `.__ module__` результирующего именованного кортежа будет присвоено это значение.
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'], module='customtypes')
point = Point(1, 2)
print(type(point))
```
```
<class 'customtypes.Point'>
```

#### Атрибуты `_fields, _fields_defaults`
Именованные кортежи имеют два дополнительных атрибута `_fields` и `_fields_default`. Первый атрибут содержит кортеж строк, в котором перечислены имена полей. Второй атрибут содержит словарь, который сопоставляет имена полей с соответствующими значениями по умолчанию, если таковые имеются.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])

tim = Person('Тимур', 29, 170)

print(tim)
print(tim._fields)
print(Person._fields)
```
```
Person(name='Тимур', age=29, height=170)
('name', 'age', 'height')
('name', 'age', 'height')
```
Неважно как обращаться к атрибуту `_fields` через переменную или через тип кортежа.

При помощи `_fields` мы можем создавать новые именованные кортежи на основании уже существующих.

Создадим именованный кортеж с именем `ExtendedPerson`, который расширяет старый кортеж `Person` новым полем `weight`.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])
ExtendedPerson = namedtuple('ExtendedPerson', [*Person._fields, 'weight'])  # распаковка полей старого кортежа

Vladik = ExtendedPerson('Владик', 29, 170, 65)

print(Vladick)
print(ExtendedPerson._fields)
```
```
ExtendedPerson(name='Владик', age=29, height=170, weight=65)
('name', 'age', 'height', 'weight')
```

Мы также можем использовать атрибут `_fields` для перебора полей и их значений с помощью встроенной функции `zip()`.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])
timur = Person('Тимур', 29, 170)
for field, value in zip(Person._fields, timur):
    print(field, '->', value)
```
```
name -> Тимур
age -> 29
height -> 170
```

С помощью атрибута `_field_defaults` мы можем выяснить, какие поля именованного кортежа имеют значения по умолчанию. Значения по умолчанию делают поля необязательными. Например, предположим, что наш именованный кортеж `Person` должен включать дополнительное поле для хранения страны, в которой живет человек. Установим дефолтное значение на Россию:
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'], defaults=['Russia'])
VlaDick = Person('Владик', 29, 170)

print(timur)
print(timur._field_defaults)
print(Person._field_defaults)
```
```
Person(name='Владик', age=29, height=170, country='Russia')
{'country': 'Russia'}
{'country': 'Russia'}
```

Если именованный кортеж не предоставляет значений по умолчанию, тогда `_field_defaults` содержит пустой словарь.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])
VliDICK = Person('Владик', 29, 170, 'Russia')

print(Person._field_defaults)
```
```
{}
```

#### Методы `_make(), _replace(), _asdict()`
Напомним, что обычные кортежи (`tuple`) имеют два встроенных метода:-
- `index()`: возвращает индекс первого элемента, значение которого равняется переданному значению
- `count()`: возвращает количество элементов в кортеже, значения которых равны переданному значению
Именованные кортежи  поддерживают все те же самые методы, но еще могут имеют свои методы. Методы, применимые только для `namedtuple` начинаются с символа нижнего подчеркивания `_make(), _replace(), _asdict()`
###### `_make()`
Метод используется для создания именованных кортежей из итерируемых объектов (список, кортеж, строка, словарь и тд)
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])
Vladick = Person._make(['Vladick', 29, 170])

print(Vladick)
```
```
Person(name='Vladik', age=29, height=170)
```
`_make()` - это метод типа, а не конкретного экземпляра, поэтому вызывать его нужно через название типа (`preson._make`). Метод работает как альтернативный конструктор типа.
###### `_asdict()`
Мы можем преобразовать именованные кортежи в словари с помощью метода `_asdict()`. Этот метод возвращает словарь, в котором имена полей используются в качестве ключей. Ключи результирующего словаря находятся  в том же порядке, что и поля в исходном именованном кортеже.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height'])
Vladik = Person._make(['Vladick', 29, 170])

print(Vladick._asdict())
```
```
{'name': 'Vladick', 'age': 29, 'height': 170}

```
###### `replace()`
Метод позволяет создавать новые именованные кортежи на основании уже существующих с заменой некоторых значений. Потребность в данном методе вызвана тем, что именованные кортежи являются неизменяемыми.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'age', 'height', 'country'])

Vlad1 = Person('Vladick', 29, 170, 'Russia')
Vlad2 = Vlad1._replace(age=30, country='Germany')

print(Vlad1)
print(Vlad2)
```

выводит:

```no-highlight
Person(name='Vladick', age=29, height=170, country='Russia')
Person(name='Vladick', age=30, height=170, country='Germany')
```