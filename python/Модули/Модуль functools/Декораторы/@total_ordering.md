#OOP #ООП #Python #total_ordering 


```python
from functools import total_ordering
```
Декоратор класса `@total_ordering` из модуля `fuctools` позволяет определить в классе лишь метод `__eq__()` и один из методов `__lt__()`, `__le__()`, `__gt__()` или `__ge__()`.  Все недостающие методы сравнения декоратор определит и реализует самостоятельно

Приведенный ниже код реализует в классе `Fruit` все возможные операции сравнения:
```python
from functools import total_ordering


@total_ordering
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __eq__(self, other):
        if isinstance(other, Fruit):
            return self.mass == other.mass
        return NotImplemented

    def __lt__(self, other):
        if isinstance(other, Fruit):
            return self.mass < other.mass
        return NotImplemented
```

Если в классе реализовано сравнение на больше(меньше), то сравнение на меньше(больше) для объектов этого класса можно считать реализованным автоматически. Аналогично себя ведут и нестрогие сравнения на больше/меньше
```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name
        self.mass = mass

    def __lt__(self, other):
        if isinstance(other, Fruit):
            return self.mass < other.mass
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)

print(fruit1 < fruit2)
print(fruit1 > fruit2)
```
```
True
False
```
