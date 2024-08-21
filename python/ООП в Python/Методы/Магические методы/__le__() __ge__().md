#OOP #ООП #Python #магический_метод #le #ge #сравнение


Магический метод `__le__()` отвечает за сравнение на меньше или равно. То есть выражение `fruit1 <= fruit2` равносильно вызову `fruit1.__le__(fruit2)`. За сравнение на больше или равно отвечает магический метод `__ge__()`, и аналогично предыдущему методу выражение `fruit1 >= fruit2` равносильно вызову `fruit1.__ge__(fruit2)`
```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __eq__(self, other):
        if isinstance(other, Fruit):
            return self.mass == other.mass
        return NotImplemented

    def __le__(self, other):
        if isinstance(other, Fruit):
            return self.mass <= other.mass
        return NotImplemented

    def __ge__(self, other):
        if isinstance(other, Fruit):
            return self.mass >= other.mass
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)
fruit3 = Fruit('груша', 150)

print(fruit1 <= fruit2)
print(fruit1 >= fruit2)
print(fruit1 <= fruit3)
print(fruit1 >= fruit3)
```
```
True
False
True
True
```