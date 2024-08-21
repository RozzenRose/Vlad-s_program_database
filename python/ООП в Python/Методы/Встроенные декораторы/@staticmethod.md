#OOP #ООП #Python #staticmethod


Декоратор для создания статических методов

Статические методы, в отличие от методов экземпляра и методов класса, не имеют обязательных параметров `self` или `cls`. Поэтому статические методы не могут изменять ни состояния объекта, ни состояния класса. Статические методы можно считать обычными функциями, которые помещены в класс для удобства. Чаще всего это какой-то вспомогательный код, предназначенный для внутреннего использования
```python
class MyClass:
    @staticmethod
    def my_staticmethod():
        print('Это статический метод')


MyClass.my_staticmethod()
```
создает в классе `MyClass` статический метод класса `my_staticmethod()` и выводит:
```
Это статический метод
```

Как мы видим, статические метод `my_staticmethod()` действительно не имеет доступа ни к классу, ни к объекту

Статические методы могут вызываться внутри методов экземпляра или класса для вычисления каких-либо значений, которые напрямую не связаны с экземпляром класса или самим классом, рассмотрим следующее определение класса `Cat`:
```python
class Cat:
    def __init__(self, name, age):
        self.name = name                                     # имя кошки
        self.age = age                                       # возраст кошки
```
Например, мы хотим определить метод `got_age()`, который мог бы возвращать возраст кошки как в кошачьих годах, так и в человеческих. Тогда мы можем определить статический метод `age_in_human_years()`, который переводит кошачьи года в человеческие, и использовать его внутри метода `get_age()`
```python
class Cat:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def get_age(self, in_human_years=False):
        if in_human_years:
            return Cat.age_in_human_years(self.age)          # переводим кошачьи года в человеческие
        return self.age

    @staticmethod
    def age_in_human_years(age):
        return (age + (age < 5) - (age > 8)) * 7             # переводим кошачьи года в человеческие


cat = Cat('Кемаль', 2)

print(cat.get_age())                                         # возраст в кошачьих годах
print(cat.get_age(True))                                     # возраст в человеческих годах
```
```
2
21
```

Статические методы можно вызвать не только через класс, но и через экземпляр:
```python
class ElectricCar:
    def __init__(self, color):
        if not ElectricCar.is_valid(color):
            raise ValueError
        self._color = color

    @staticmethod
    def is_valid(data):
        return isinstance(data, str) and data.isalpha()


car = ElectricCar('black')

print(car.is_valid('red'))
print(ElectricCar.is_valid('yellow'))
```
```
True
True
```