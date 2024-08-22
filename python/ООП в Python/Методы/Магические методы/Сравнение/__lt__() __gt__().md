#OOP #ООП #Python #магический_метод #lt #gt #сравнение


Аналогично сравнению на равенство, данные операции реализуются с помощью соответствующих магических методов

Рассмотрим класс `Fruit`, описывающий фрукт, и создадим два экземпляра этого класса:
```python
class Fruit:
    def __init__(self, name, mass):
        self.name = name                          # название фрукта
        self.mass = mass                          # масса фрукта в граммах

    def __eq__(self, other):
        if isinstance(other, Fruit):
            return self.mass == other.mass        # два фрукта равны, если равны их массы
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)
```
В данном классе определено сравнение на равенство по следующему правилу: два фрукта считаются равными, если равны их массы. Однако масса является численной величиной, поэтому помимо равенства мы можем определить, например, какой их фруктов меньше(больше), в зависимости от того, чья масса меньше(больше). Для этого нам потребуется реализовать магические методы `__lt__()` и `__gt__()`

Магический метод `__lt__()` отвечает за сравнение на меньше. То есть выражение `fruit1 < fruit2` равносильно вызову `fruit1.__lt__(fruit2)`. За сравнение на больше отвечает магический метод `__gt__()`, и аналогично предыдущему методу выражение `fruit1 > fruit2` равносильно вызову `fruit1.__gt__(fruit2)`
```python
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

    def __gt__(self, other):
        if isinstance(other, Fruit):
            return self.mass > other.mass
        return NotImplemented


fruit1 = Fruit('банан', 150)
fruit2 = Fruit('яблоко', 180)

print(fruit1 < fruit2)
print(fruit1 > fruit2)
print(fruit2 < fruit1)
print(fruit2 > fruit1)
```
```
True
False
False
True
```
