#OOP #ООП #Python #магический_метод #атрибуты #delattr


Метод `__delattr__()` вызывается при удалении атрибута
```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed
    
    def __delattr__(self, attr):
        print(f'Удаляю атрибут {attr}')
        del self.__dict__[attr]


cat = Cat('Никся', 'Девон рекс')

del cat.name
print(cat.__dict__)

del cat.breed
print(cat.__dict__)
```
```
Удаляю атрибут name
{'breed': 'Девон рекс'}
Удаляю атрибут breed
{}
```
Аналогично методу `__setattr__()`, удаление атрибута внутри метода `__delattr__()` происходит напрямую через словарь атрибутов `__dict__`

Вместо обращения к словарю атрибутов `__dict__`, метод `__delattr__()` может использовать свою базовую реализацию из класса `object`
```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed
    
    def __delattr__(self, attr):
        print(f'Удаляю атрибут {attr}')
        object.__delattr__(self, attr)


cat = Cat('Никся', 'Девон рекс')

del cat.name
print(cat.__dict__)

del cat.breed
print(cat.__dict__)
```
```
Удаляю атрибут name
{'breed': 'Девон рекс'}
Удаляю атрибут breed
{}
```