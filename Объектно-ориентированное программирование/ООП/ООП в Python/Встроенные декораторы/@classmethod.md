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
На самом деле мы уже не раз сталкивались с методами класса, например, когда изучали такие типы данных как `dict` и `datetime`
```python
from datetime import datetime

cats = dict.fromkeys(['Кемаль', 'Роджер'])                   # словарь со значениями по умолчанию
dt = datetime.strptime('12.10.2022', '%d.%m.%Y')             # дата на основе строки

print(cats)
print(dt)
```
```
{'Кемаль': None, 'Роджер': None}
2022-10-12 00:00:00
```