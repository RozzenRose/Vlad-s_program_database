#OOP #ООП #Python 


Для реализации слотов в `Python` используется атрибут класса `__slots__`. С помощью данного атрибута мы определяем набор атрибутов, которыми может обладать экземпляр класса

Например, если экземпляры нашего класса `Point`, описывающего точку на плоскости, всегда содержат только два атрибута - координаты точки по осям `x` и `y`, то мы можем указать данные атрибуты в слотах следующим образом
```python
class Point:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y
```
В качестве значения `__slots__` мы указываем кортеж, содержащей строковые имена атрибутов, которыми может обладать экземпляр класса `Point`. Следует отметить, что имена атрибутов, помимо принадлежности к типу `str`, должны быть корректными с точки зрения правил именования атрибутов в `Python`

Значением атрибута `__slots__` может быть любой итерируемый объект, однако на практике используют кортеж

После определения слотов в классе `Python` не будет использовать словарь `__dict__` для управления атрибутами экземпляров этого класса
```python
class Point:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y


point = Point(1, 2)

print(point.__dict__)
```
```
AttributeError: 'Point' object has no attribute '__dict__'. Did you mean: '__dir__'?
```
Так как `__slots__` является обыкновенным атрибутом класса, обращаться к нему можно как через сам класс, так и через его экземпляры
```python
class Point: 
	__slots__ = ('x', 'y') 
	
	def __init__(self, x, y): 
		self.x = x
        self.y = y

point = Point(1, 2)

print(Point.__slots__)
print(point.__slots__)
```
```
('x', 'y')
('x', 'y')
```

### Слоты при наследовании
Рассмотрим поведение слотов при наследовании. Определим родительский класс `Point2D`, описывающий точку на плоскости, и его дочерний класс `Point3D`, описывающий точку в пространстве
```python
class Point2D:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

class Point3D(Point2D):
    def __init__(self, x, y, z):
        super().__init__(x, y)
        self.z = z
```
Класс `Point3D` не имеет слотов, поэтому его экземпляры имеют словарь атрибутов `__dict__`. Таким образом, дочерний класс `Point3D` использует как слоты из родительского класса, так и словарь атрибутов `__dict__`
```python
class Point2D:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

class Point3D(Point2D):
    def __init__(self, x, y, z):
        super().__init__(x, y)
        self.z = z


point = Point3D(1, 2, 3)

print(point.x, point.y, point.z)
print(point.__slots__)
print(point.__dict__)
```
```
1 2 3
('x', 'y')
{'z': 3}
```
Такое поведение разрешает нам определять дополнительные атрибуты для экземпляров дочернего класса. Если мы хотим, чтобы дочерний класс также использовал слоты, мы можем определить их в нем самом

Определяя слоты в дочернем классе, дублировать слоты родительского класса не нужно

Теперь экземпляры класса `Point3D` будут использовать слоты для всех своих атрибутов `x`, `y` и `z`, при этом не будет иметь словарь атрибутов `__dict__`
```python
class Point2D:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

class Point3D(Point2D):
    __slots__ = ('z',)

    def __init__(self, x, y, z):
        super().__init__(x, y)
        self.z = z


point = Point3D(1, 2, 3)

print(point.__dict__)
```
```
AttributeError: 'Point3D' object has no attribute '__dict__'. Did you mean: '__dir__'?
```

### Производительность
Одной из причин использования слотов может быть желание увеличить производительность класса путем ускорения выполнения некоторых операций и снижения потребляемой памяти
#### Доступ к атрибутам 
Для сравнения скорости доступа к атрибутом воспользуемся функцией `repeat()` из модуля `timeit`
```python
from timeit import repeat

class PointWithoutSlots:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class PointWithSlots:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

def get_set_test(cls):
    obj = cls(0, 0)
    def get_set():
        obj.x += 1
        obj.y += 2
    return get_set


print(max(repeat(get_set_test(PointWithoutSlots))))    # результат класса без слотов
print(max(repeat(get_set_test(PointWithSlots))))       # результат класса со слотами
```
```
0.11013359995558858
0.09572770004160702
```
Класс с использованием слотов в среднем на `15-20%` быстрее в операциях к атрибутам.
#### Потребляемая память
Для сравнения потребляемой памяти воспользуемся функцией `asizeof()` из модуля `pympler.asizeof`
```python
from pympler import asizeof

class PointWithoutSlots:
    def __init__(self, x, y):
        self.x = x
        self.y = y

class PointWithSlots:
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y


point1 = PointWithoutSlots(0, 0)
point2 = PointWithSlots(0, 0)

print(asizeof.asizeof(point1))                         # результат класса без слотов
print(asizeof.asizeof(point2))                         # результат класса со слотами
```
```
288
72
```
Экземпляры классов с использованием слотов занимают меньше памяти, чем экземпляры классов без использования слотов, Значительную экономию памяти можно получить в ситуациях, когда создается большое количество экземпляров классов

### Примечания
##### Примечание 1 
Прочитать про использование атрибута `__slots__` можно по  [ссылке](https://wiki.python.org/moin/UsingSlots%C2%A0)
##### Примечание 2
Если при наследовании родительский класс не использует слоты, а дочерний класс использует, экземпляры дочернего класса все равно будут иметь словарь атрибутов `__dict__`
```python
class Shape:
    pass

class Point(Shape):
    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y


point = Point(1, 2)

print(point.__slots__)
print(point.__dict__)

point.color = 'yellow'
print(point.__dict__)
```
```
('x', 'y')
{}
{'color': 'yellow'}
```