#метод #itertools #python #dropwhile

Возвращает итератор, который генерирует элементы их входного итерируемого сразу же после того, как для заданного условия будет получено ложное значение
Аргументы функции:
- `predicate` — фильтрующая функция, возвращающая `bool` значение
- `iterable` — итерируемый объект
```python
from itertools import dropwhile

numbers = [1, 1, 2, 3, 4, 4, 5, 6, 7, 8, 9, 10, 1, 2, 3]
words = ['is', 'an', 'of', 'python', 'C#', 'beegeek', 'is']

new_numbers = list(dropwhile(lambda num: num <= 5, numbers))
print(new_numbers)

for word in dropwhile(lambda s: len(s) == 2, words):
    print(word)
```
```
[6, 7, 8, 9, 10, 1, 2, 3]
python
C#
beegeek
is
```
Если требуется обеспечить более сложную логику фильтрации, то вместо использования лямбда функции нужно определить функцию явно
```python
from itertools import dropwhile

def should_drop(x):
    print('Testing:', x)
    return x < 1

for i in dropwhile(should_drop, [-1, 0, 1, 2, -2]):
    print('Yielding:', i)
```
```
Testing: -1
Testing: 0
Testing: 1
Yielding: 1
Yielding: 2
Yielding: -2
```
Функция `fropwhile()` примерно эквивалентна следующему коду:
```python
def dropwhile(predicate, iterable):
    iterable = iter(iterable)
    for x in iterable:
        if not predicate(x):
            yield x
            break
    for x in iterable:
        yield x
```