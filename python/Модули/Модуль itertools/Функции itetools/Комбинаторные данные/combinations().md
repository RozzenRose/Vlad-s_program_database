#метод #itertools #python #combinations


Функция `combinations()` возвращает итератор, который содержит все сочетания их элементов переданного итерируемого объекта. Каждое сочетание заключено в кортеж нужной длины.

Аргументы функции:
- `iterable` — итерируемый объект
- `r` — целое число, длина возвращаемых кортежей
```python
from itertools import combinations

numbers = [1, 2, 3, 4]

print(list(combinations(numbers, r=1)))
print(list(combinations(numbers, r=2)))
print(list(combinations(numbers, r=3)))
print(list(combinations(numbers, r=4)))
```
```
[(1,), (2,), (3,), (4,)]
[(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]
[(1, 2, 3), (1, 2, 4), (1, 3, 4), (2, 3, 4)]
[(1, 2, 3, 4)]
```
Сочетания генерируются в том порядке, в котором элементы стоят в переданном итерируемом объекте
```python
from itertools import combinations

letters = 'dbca'

print(list(combinations(letters, 1)))
print(list(combinations(letters, 2)))
print(list(combinations(letters, 3)))
print(list(combinations(letters, 4)))
```
```
[('d',), ('b',), ('c',), ('a',)]
[('d', 'b'), ('d', 'c'), ('d', 'a'), ('b', 'c'), ('b', 'a'), ('c', 'a')]
[('d', 'b', 'c'), ('d', 'b', 'a'), ('d', 'c', 'a'), ('b', 'c', 'a')]
[('d', 'b', 'c', 'a')]
```
Обратите внимание на то, что элементы итерируемого объекта рассматриваются как уникальные в зависимости от их положения, а не от их значения. Поэтому, если элементы уникальны, повторных значений в каждом сочетании не будет.
```python
from itertools import combinations

numbers = [1, 2, 2, 2]

print(list(combinations(numbers, 2)))
```
```no-highlight
[(1, 2), (1, 2), (1, 2), (2, 2), (2, 2), (2, 2)]
```