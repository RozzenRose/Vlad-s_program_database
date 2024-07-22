#метод #itertools #python #filterfalse 

Функция `filterfalse()` возвращает итератор, который генерирует элементы из входного итерируемого объекта для которых заданное условие ложно. По сути, действия функции `filterfalse()` противоположены действиям встроенной функции `filter()`

Аргументы функции:
- `predicate` — фильтрующая функция, возвращающая `bool` значение
- `iterable` — итерируемый объект

```python
from itertools import filterfalse

numbers = [1, 1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3]
words = ['is', 'an', 'of', 'python', 'C#', 'beegeek', 'is']

new_numbers = list(filterfalse(lambda num: num <= 5, numbers))
print(new_numbers)

for word in filterfalse(lambda s: len(s) == 2, words):
    print(word)
```
```
[6, 7, 8, 9, 10]
python
beegeek
```

Если требуется обеспечить более сложную логику фильтрации, то вместо использования лямбда функции нужно определить функцию явно
```python
from itertools import takewhile

def should_take(x):
    print('Testing:', x)
    return x < 2

for i in takewhile(should_take, [-1, 0, 1, 2, -2]):
    print('Yielding:', i)
```
```
Testing: -1
Yielding: -1
Testing: 0
Yielding: 0
Testing: 1
Yielding: 1
Testing: 2
```

Как только функция `should_take()` возвращает ложное значение, функция `takewhile()` прекращает обработку переданного итерируемого объекта
```python
def takewhile(predicate, iterable):
    for x in iterable:
        if predicate(x):
            yield x
        else:
            break
```