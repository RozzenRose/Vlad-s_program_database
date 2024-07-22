#метод #itertools #python #accumulate

Функция `accumulate()` возвращает итератор, элементами которого являются накопленные суммы или накопленные результаты функции `func`

Аргумента функции:
- `iterable` — итерируемый объект
- `func` — функция, принимающая два аргумента, по умолчанию используется функция сложения `operator.add`
- `initial` — начальное значение, по умолчанию имеет значение `None`

Функция работает аналогично функции `reduce()` из модуля `functools` за тем исключением, что функция `acculate()` генерирует все промежуточные результаты, а не только конечный
```python
from itertools import accumulate
import operator

data = [3, 4, 6, 2, 1, 9, 0, 7, 5, 8]

print(list(accumulate(data)))
print(list(accumulate(data, operator.mul)))
print(list(accumulate(data, max)))
print(list(accumulate(data, min)))
```
```
[3, 7, 13, 15, 16, 25, 25, 32, 37, 45]
[3, 12, 72, 144, 144, 1296, 0, 0, 0, 0]
[3, 4, 6, 6, 6, 9, 9, 9, 9, 9]
[3, 3, 3, 2, 1, 1, 0, 0, 0, 0]
```
Обычно количество элементов результирующего итератора совпадает с количеством элементов итерируемого объекта. Однако, если задано значение аргумента `initial`, то накопление начинается с начального значения `initial`, и в этом случае результирующий итератор будет иметь один дополнительный элемент
```python
from itertools import accumulate

print(list(accumulate([1, 2, 3, 4, 5], initial=100)))
```
```
[100, 101, 103, 106, 110, 115]
```

Функция `accumulate()`примерно эквивалентна следующему коду:
```python
import operator

def accumulate(iterable, func=operator.add, *, initial=None):
    it = iter(iterable)
    total = initial
    if initial is None:
        try:
            total = next(it)
        except StopIteration:
            return
    yield total
    for element in it:
        total = func(total, element)
        yield total
```