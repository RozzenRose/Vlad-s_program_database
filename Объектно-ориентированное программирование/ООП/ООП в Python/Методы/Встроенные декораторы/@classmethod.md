#OOP #ООП #python #classmethod


Декоратор `@classmethod` используется для создания методов класса
```python
class MyClass:
    @classmethod
    def my_classmethod(cls):
        print('Это метод класса')
        print(cls)


MyClass.my_classmethod()
```
создает в классе `MyClass` метод класса `my_classmethod()` и выводит:
```no-highlight
Это метод класса
<class '__main__.MyClass'>
```

Зачастую методы класса используются для добавления альтернативного способа создания экземпляров класса. Рассмотрим следующее определение класса `Cat`:
```python
class Cat:
    def __init__(self, breed, name):
        self.breed = breed                                   # порода кошки
        self.name = name                                     # имя кошки
```
Экземпляры данного класса имеют два атрибута - породу и имя. Скажем, мы заметили, что очень часто имеем дело с кошками британской породы и решили, что было бы удобно создавать объекты класса `Cat`, указывая лишь имя, а в качестве породы по умолчанию устанавливая `Британский`. Для этого мы можем реализовать метод класса, который принимает лишь имя кошки и возвращает объект класса `Cat` британской породы с указанным именем
```python
class Cat:
    def __init__(self, breed, name):
        self.breed = breed
        self.name = name

    @classmethod
    def british(cls, name):
        return cls('Британский', name)                       # равнозначно Cat('Британский', name)


cat = Cat.british('Кемаль')

print(cat.breed, cat.name)
```
```
Британский Кемаль
```

Метод класса, имеет доступ к атрибутам класса и может изменить их состояние:
```python
class ElectricCar:
    status = True

    @classmethod
    def disable(cls):
        cls.status = False


car1 = ElectricCar()
car2 = ElectricCar()

print(car1.status, car2.status)

ElectricCar.disable()

print(car1.status, car2.status)
```
```
True True
False False
```

Но так же изменить атрибут класса можно через атрибут `__class__` экземпляра класса.
```python
class ElectricCar:
    status = True

    def disable(self):
        self.__class__.status = False


car1 = ElectricCar()
car2 = ElectricCar()

print(car1.status, car2.status)

car1.disable()

print(car1.status, car2.status)
```
```
True True
False False
```