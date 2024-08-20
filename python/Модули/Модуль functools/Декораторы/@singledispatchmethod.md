#OOP #ООП #Python #singledispatchmethod


```python
from functools import singledispatchmethod
```
Декоратор `@singledispatchmethod` позволяет создавать универсальную функцию одиночной диспетчеризации. Что позволяет определить несколько инициализаторов и выборочно их использовать в зависимости от типа первого переданного в них аргумента

***Универсальная функция*** (generic function) представляет собой функцию, составленную их нескольких функций, реализующих одну и ту же операцию для различных типов. Одиночная диспетчеризация (single dispatch) - это алгоритм, который выбирает нужную реализацию на основе типа ***одного аргумента***. Именно поэтому диспетчеризация называется одиночной

Создание универсальной функции одиночной диспетчеризации происходит с помощью декоратора `@singledspachmethod` из модуля `functools`, использование которого синтаксически схоже с использованием декоратора `@property`

```python
from functools import singledispatchmethod


class MyClass:
    @singledispatchmethod
    def base_implementation(self, arg):
        print('Базовая реализация')

    @base_implementation.register(int)
    def int_implementation(self, arg):
        print('Реализация для целочисленного аргумента')

    @base_implementation.register(str)
    def str_implementation(self, arg):
        print('Реализация для строкового аргумента')


obj = MyClass()

obj.base_implementation(1)
obj.base_implementation('bee')
obj.base_implementation([1, 2, 3])
```
определяет метод `base_implementation()`, имеющий две альтернативные реализации, и выводит:
```
Реализация для целочисленного аргумента
Реализация для строкового аргумента
Базовая реализация
```
В параметре выше мы сначала определяем метод `base_implemintation()`, представляющий базовую реализацию, и применяем к нему декоратор `@singledispatchmethod`. Затем мы определяем метод `int_implementation()`, который является альтернативной реализацией метода `base_implementation()`, если в него будет передан целочисленный аргумент, и декорируем его с помощью `@base_implementation.register` с соответствующим аргументом `int`. Другими словами декоратор `@base_implementation.register` закрепляет за методом `base_implementation()` метод `int_implementation()` и вызывает его, если передаваемый в него аргумент принадлежит типу `int`. Далее аналогично определяем альтернативную реализацию для строкового аргумента.

При вызове метода `base_implementation()` нужная реализация выбирается на основе типа аргумента, следующего после `self`. Если аргумент принадлежит типу `int`, будет вызван метод `int_inplementation()`, во всех остальных случаях - метод `base_implementation()`

При использовании декоратор `@singledispatchmethod` альтернативные реализации не должны иметь то же имя, что и базовая версия

Используя данный способ для перегрузки метода `__init__()`, мы можем сымитировать наличие в классе нескольких инициализаторов
```python
from functools import singledispatchmethod


class Cat:
    @singledispatchmethod
    def __init__(self, breed, name, age):
        self.breed = breed
        self.name = name
        self.age = age

    @__init__.register(list)
    def _from_list(self, data):
        self.breed, self.name, self.age = data


cat1 = Cat('Британский', 'Кемаль', 1)                         # передаем все данные отдельно
cat2 = Cat(['Манчкин', 'Роджер', 1])                          # передаем список с данными

print(cat1.breed, cat1.name, cat1.age)
print(cat2.breed, cat2.name, cat2.age)
```
```
Британский Кемаль 1
Манчкин Роджер 1
```
Имена методов, являющихся альтернативными, зачастую предваряют одним символом нижнего подчеркивания, тем самым делая их защищенными

 С помощью декоратора `@singledispatchmethod` можно перегружать любые методы в классе, будь то методы экземпляра, методы класса или статические методы.
```python
from functools import singledispatchmethod


class MyClass:
    @singledispatchmethod
    def method(self, arg):
        print('Базовая реализация метода экземпляра')

    @method.register(int)                                     # перегружаем метод экземпляра
    def _method(self, arg):
        print('Реализация метода экземпляра для целочисленного аргумента')

    @singledispatchmethod
    @classmethod
    def class_method(cls, arg):
        print('Базовая реализация метода класса')

    @class_method.register(str)                               # перегружаем метод класса
    @classmethod
    def _class_method(cls, arg):
        print('Реализация метода класса для строкового аргумента')

    @singledispatchmethod
    @staticmethod
    def static_method(arg):
        print('Базовая реализация статического метода')

    @static_method.register(list)                             # перегружаем статический метод
    @staticmethod
    def _static_method(arg):
        print('Реализация статического метода для списочного аргумента')


obj = MyClass()

obj.method('bee')
obj.method(1)
print()

obj.class_method(1)
obj.class_method('bee')
print()

obj.static_method(1)
obj.static_method(['b', 'e', 'e'])

```
```
Базовая реализация метода экземпляра
Реализация метода экземпляра для целочисленного аргумента

Базовая реализация метода класса
Реализация метода класса для строкового аргумента

Базовая реализация статического метода
Реализация статического метода для списочного аргумента
```