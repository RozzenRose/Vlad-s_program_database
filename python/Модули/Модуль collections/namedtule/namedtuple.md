#collections #Python #именнованныe_кортежи #namedtuple #объект #_fields #_fields_default #_make #_replace #_asdict


Именованные кортежи `namedtuple` - это подтип обычных кортежей в Python. У них те же функции, что и у обычных, но их значения можно получать как с помощью индекса, так и с помощью имени через точку.

Основное предназначение этого типа - улучшение читаемости кода.

Допустим у нас есть точка на координатной плоскости, имеющая два координаты `x` и `y`.
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])     # объявляем тип Point именованного кортежа

point = Point(3, 7)                         # создаем именованный кортеж Point

print(point)
print(point.x, point.y)
print(point[0], point[1])
print(type(point))
```
```
Point(x=3, y=7)
3 7
3 7
<class '__main__.Point'>
```
В этом коде мы создали объект типа Point, который является именованным кортежем, к любому элементу которого можно обратиться, как через точку(`point.x`, `point.y`), так и по индексу (`point[0]`, `point[1]`).

Эти кортежи неизменяемые, как и обычные кортежи, но если внутри того кортежа хранятся изменяемые объекты, то их можно изменять.
```python
from collections import namedtuple

Person = namedtuple('Person', ['name', 'children'])

sveta = Person('Sveta Gueva', ['Larisa', 'Timur'])
print(sveta)

sveta.children.append('Soslan')
print(sveta)
```
```
Person(name='Sveta Gueva', children=['Larisa', 'Timur'])
Person(name='Sveta Gueva', children=['Larisa', 'Timur', 'Soslan'])
```

Кортежи и именованные кортежи с изменяемыми значениями не могут быть хешированы, поэтому не могут быть элементами множеств и ключами в словарях. Кортежи и именованные кортежи без изменяемых значений могут быть хешированы, поэтому могут быть элементами множеств и ключами в словарях.

Так же к элементам именованного кортежа можно обращаться не только с помощью позиционных аргументов, но и с помощью именованных:
```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
point1 = Point(2, 4)                          # позиционные аргументы
point2 = Point(y=10, x=3)                     # именованные аргументы

print(point1)
print(point2)
```
```
Point(x=2, y=4)
Point(x=3, y=10)
```
### [[Методы nametuple]]