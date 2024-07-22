#метод #itertools #python #takewhile

Функция `takewhile()` возвращает итератор, который генерирует элементы из выходного итерируемого объекта до тех пор пока для заданного условия не будет получено ложное значение. По сути, действия функции `takewhile()` противоположны действиям функции `dropwhile()`

Аргументы функции:
- `predicate` — фильтрующая функция, возвращающая `bool` значение
- `iterable` — итерируемый объект

```python
from itertools import takewhile

numbers = [1, 1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3]
words = ['is', 'an', 'of', 'python', 'C#', 'beegeek', 'is']

new_numbers = list(takewhile(lambda num: num <= 5, numbers))
print(new_numbers)

for word in takewhile(lambda s: len(s) == 2, words):
    print(word)
```
```
[1, 1, 2, 3, 4, 4, 5]
is
an
of
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

Функция `takewhile()` примерно эквивалентна следующему коду:
```python
def takewhile(predicate, iterable):
    for x in iterable:
        if predicate(x):
            yield x
        else:
            break
```
