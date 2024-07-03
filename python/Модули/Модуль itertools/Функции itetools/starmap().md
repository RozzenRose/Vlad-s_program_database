#метод #itertools #python #starmap

Функция `starmap()` используется вместо `map()` в том случае, когда элементами итерируемого объекта являются другие итерируемые объекты, скажем, кортежи (`tuple`), и каждый элемент этих кортежей должен быть передан в функцию `function` в качестве самостоятельного аргумента
```python
from itertools import starmap

persons = [('Rozzen', 'Rose'), ('Wlad', 'Belsky')]
pairs = [(1, 3), (2, 5), (6, 4)]
points = [(1, 1, 1), (1, 1, 2), (2, 2, 3)]

full_names = list(starmap(lambda name, surname: f'{name} {surname}', persons))

print(full_names)
print(*starmap(lambda a, b: a + b, pairs))
print(*starmap(lambda x, y, z: x * y * z, points))
```
```
['Rozzen Rose', 'Wlad Belsky']
4 7 10
1 2 12
```
Разница между функциями `map()` и `starmap()` заключается в способе передачи аргументов вызываемой функции `function` и аналогична разнице между `function(a, b)` и `function(*c)`

Функция `starmap()` примерно аналогична следующему коду:
```python
def starmap(function, iterable):
    for args in iterable:
        yield function(*args)
```