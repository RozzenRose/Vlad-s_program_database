#OOP #ООП #Python #магический_метод #set_name


Магические метод `__set_name__()` позволяет неявно использовать имя переменной в качестве имени атрибута, за которым будет закреплен дескриптор.
```python
class NonEmptyString:
    def __set_name__(self, cls, attr):
        self._attr = attr

    def __get__(self, obj, cls):
        if self._attr in obj.__dict__:
            return obj.__dict__[self._attr]
        else:
            raise AttributeError('Атрибута не существует')

    def __set__(self, obj, value):
        if isinstance(value, str) and len(value) > 0:
            obj.__dict__[self._attr] = value
        else:
            raise ValueError('Некорректное значение')
        
    def __delete__(self, obj):
        del obj.__dict__[self._attr]

class Cat:
    name = NonEmptyString() #строка 'name' автоматически передается в метод __set_name__()

    def __init__(self, name):
        self.name = name


cat = Cat('Кемаль')
print(cat.name)

cat.name = 'Роджер'
print(cat.name)

del cat.name
print(hasattr(cat, 'name'))
```
```
Кемаль
Роджер
False
```
При выполнении строки `name = NonEmptyString()`, этот самый `name` будет передан в метод `__set_name__()`, и атрибут тоже будет называться `name`

Стоит обратить внимание на аргументы, передаваемые в метод `__set_name__()`, ими являются класс, в котором создается дескриптор, и имя переменной, которой присваивается дескриптор.
```python
class NonEmptyString:
    def __set_name__(self, cls, attr):
        print(cls)
        print(attr)
        self._attr = attr

    def __get__(self, obj, cls):
        if self._attr in obj.__dict__:
            return obj.__dict__[self._attr]
        else:
            raise AttributeError('Атрибута не существует')

    def __set__(self, obj, value):
        if isinstance(value, str) and len(value) > 0:
            obj.__dict__[self._attr] = value
        else:
            raise ValueError('Некорректное значение')
        
    def __delete__(self, obj):
        del obj.__dict__[self._attr]

class Cat:
    name = NonEmptyString()

    def __init__(self, name):
        self.name = name
```
```
<class '__main__.Cat'>
name
```
