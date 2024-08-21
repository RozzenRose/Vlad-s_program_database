#OOP #ООП #Python #property


Рассмотрим следующее определение класса `Cat`:
```python
class Cat:
    def __init__(self, name):
        self._name = name                              # имя кошки

    def get_name(self):                                # геттер, используется для получения имени
        return self._name

    def set_name(self, name):                          # сеттер, используется для изменения имени
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')
```
Экземпляр данного класса содержит атрибут `_name` и два метода `get_name()` и `set_name()`, позволяющий работать с этим атрибутом. Если мы решим добавить еще один атрибут экземпляру нашего класса, нам снова потребуется определить для него два метода - геттер и сеттер

Несмотря на то что геттеры и сеттеры даже в таком виде выполняют свою основную задачу, работать с атрибутами с помощью них становится несколько сложнее, так как атрибуты перестают быть атрибутами как таковыми - мы обращаемся к ним и изменяем их значения с помощью методов. Более того, для каждого атрибута нужно помнить оба его метода

Самый популярный способ упростить работу с атрибутами, не потеряв все преимущества геттеров и статтеров - превратить их в `свйоства`. Свойства предоставляют промежуточную функциональность между атрибутами и методами. Другими словами, они позволяют создавать методы, которые ведут себя как атрибуты

В `Python` свойства определяются как атрибуты классов, за которыми закрепляются соответствующие геттеры, сеттеры и делитеры. Так как свойства являются атрибутами класса, они доступны всем экземплярам этого класса

Для создания свойств используется встроенная функция `property()`. Она принимает четыре аргумента:
- `fget` — функция для получения значения атрибута
- `fset` — функция для установки значения атрибута
- `fdel` — функция для удаления атрибута
- `doc` — строка документации

Функция `property()` возвращает специальный объект `property` - свойство на основе переданных геттера, сеттера и делитера.

Все четыре аргумента функции `property()` являются необязательными по умполчанию имеют значение `None`

Приведенный ниже код определяет свойство `name`, за которым закреплены геттер `get_name()` и сеттер `set_name()`:
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)                # создаем свойство name для управления именем
```
Теперь работать с атрибутом `_name` можно через свойство `name`. При этом во время обращения к свойству `name` как к атрибуту будет неявно вызываться метод `get_name()`, а во время установки значения свойству `name` как атрибуту будет неявно вызываться метод `set_name()`
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


cat1 = Cat('Кемаль')
cat2 = Cat('Роджер')

print(cat1.name)                                       # равнозначно cat1.get_name()
print(cat2.name)                                       # равнозначно cat2.get_name()
```
```
Кемаль
Роджер
```
Чтобы убедиться в том, что обращение к свойству `name` неявно вызывает метод `get_name()`, мы можем добавить в данный метод вывод сообщения
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        print(f'Возвращаю имя {self._name}')
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


cat1 = Cat('Кемаль')
cat2 = Cat('Роджер')

print(cat1.name)
print(cat2.name)
```
```
Возвращаю имя Кемаль
Кемаль
Возвращаю имя Роджер
Роджер
```
Как уже было сказано выше, при установке значения свойству `name` неявно вызывается метод `set_name()`
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


cat1 = Cat('Кемаль')
cat2 = Cat('Роджер')

cat1.name = 'Рэтчет'                                   # равнозначно cat1.set_name('Рэтчет')

print(cat1.name)
print(cat2.name)
```
```
Рэтчет
Роджер
```

Чтобы убедиться в том, что установка значения свойства `name` неявно вызывает метод `set_name()`, мы можем попытаться установить некорректное имя
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


cat = Cat('Кемаль')

cat.name = 1                                           # равнозначно cat.set_name(1)
```
```
ValueError: Некорректное имя
```
Поскольку все аргументы функции `property()` являются необязательными, мы можем определять свойства, которые, например, доступны только для чтения. При попытке изменить значения такого свойства будет возбуждено исключение.


### Объекты property
Свойства являются атрибутами классов, которые доступны всем экземплярам этих классов. Обращаясь к свойству через экземпляр класса, мы работаем с ним как с атрибутом, неявно вызывая соответствующие методы (геттер, сеттер и делитер). При этом само свойство является обычным объектом, и мы можем получить к нему доступ через класс, в котором это свойство определено
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


print(Cat.name)
print(type(Cat.name))
```
```
<property object at 0x000001829C8F56C0>
<class 'property'>
```
Таким образом, свойство - это атрибут класса, который управляет атрибутами экземпляров. Мы также можем считать свойство набором методов (геттер, сеттер, делитер), собранных вместе в единый объект. Обратиться к методам, содержащимся в свойстве, можно через атрибуты `fget`, `fset` и `fdel`
```python
class Cat:
    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    def set_name(self, name):
        if isinstance(name, str) and name.isalpha():
            self._name = name
        else:
            raise ValueError('Некорректное имя')

    name = property(get_name, set_name)


print(Cat.name.fget)                                   # обращаемся к геттеру свойства
print(Cat.name.fset)                                   # обращаемся к сеттеру свойства
print(Cat.name.fdel)                                   # обращаемся к делитеру свойства
```
```
<function Cat.get_name at 0x000001F8C2FC1090>
<function Cat.set_name at 0x000001F8C2FC1120>
None
```
`property` — это класс, предназначенный для работы как функция, а не как обычный класс, поэтому большинство разработчиков называют ее функцией. По этой же причине `property()` не соответствует соглашению Python об именовании классов.
