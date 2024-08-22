#OOP #ООП #Python #магический_метод #ne #eq #сравнение


Для возможности сравнивать экземпляры класса `Point` определим в нем метод `__eq__()`. Будем считать точки равными, если они имеют равные координаты по обеим осям
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 == p2)
print(p1 == p3)
print(p2 == p3)
```
```
True
False
False
```
Как мы видим, теперь равенство объектов определяется иначе, а именно на основании их координат. Точка `p1` равна точке `p2`, так как их координаты совпадают, в то время как точки `p1` и `p2` не равны точке `p3`, так как их координаты не совпадают

Однако определенный нами метод `__eq__()` работает верно лишь в том случае, когда мы сравниваем объект типа `Point` с объектом такого же типа. Если мы попытаемся сравнить точку `p1`, скажем, с кортежем, мы получим ошибку
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


p1 = Point(1, 2)

print(p1 == (1, 2))
```
```
AttributeError: 'tuple' object has no attribute 'x'
```

Чтобы исправить это, мы можем добавить в метод `__eq__()` проверку на то, что объект, с которым происходит сравнение, принадлежит необходимому типу. В противном случае метод может возбуждать исключение или возвращать значение `False`
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p1 = Point(1, 2)

print(p1 == (1, 2))
```
```
False
```

Аналогично сравнению на равенство для сравнения на неравенство с помощью оператора `!=` используется магический метод `__ne__()`
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False

    def __ne__(self, other):
        if isinstance(other, Point):
            return self.x != other.x or self.y != other.y
        return True


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 != p2)
print(p1 != p3)
print(p2 != p3)
```
```
False
True
True
```
Важно отметить. что в `Python` автоматически реализует метод `__ne__()`, если метод `__eq__()` уже реализован
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 != p2)
print(p1 != p3)
print(p2 != p3)
```
```
False
True
True
```
Однако если нам требуется несколько иная реализация, мы всегда можем определить метод `__ne__()` вручную

При реализованном методе `__ne__()` метод `__eq__()` автоматически не реализуется

 Метод `__eq__()`, помимо сравнения с помощью оператора `==`, также вызывается при проверке на принадлежность с помощью оператора `in`.
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        print('Вызов метода __eq__()')
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return False


p = Point(2, 2)        
points = [Point(1, 1), Point(2, 2), Point(3, 3)]

print(p in points)
```
```
Вызов метода __eq__()
Вызов метода __eq__()
True
```

### Константа NotImplemented
При сравнении двух объектов с помощью оператора `==` `Python` автоматически вызывает метод `__eq__()` у первого объекта, передавая методу в качестве аргумента второй. Однако если мы попробуем вызывать метод `__eq__()` у объекта, в классе которого данный метод не реализован, вместо значений `True` или `False` мы получим значение `NotImplemented`
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y


p1 = Point(1, 2)
p2 = Point(1, 2)

print(p1.__eq__(p2))
```
```
NotImplemented
```
Дело в том, что когда мы пытаемся два объекта с помощью оператора `==`, например, `p1` и `p2`, `Python` сначала вызывает метод `__eq__()` у первого объекта `p1`, и если метод возвращает значение `True` или `False`, оно становится результатом сравнения, однако если метод вновь возвращает `NotImplemented`, результатом сравнения становится значение `False`, так как `p1` не знает, как сравнивать себя с `p2`, как и `p2`не знает, как сравнить себя с `p1`
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        if isinstance(other, Point):
            return self.x == other.x and self.y == other.y
        return NotImplemented


p1 = Point(1, 2)
p2 = Point(1, 2)

print(p1 == p2)
print(p1 == None)
print(p1 == (1, 2))
```
```
True
False
False
```