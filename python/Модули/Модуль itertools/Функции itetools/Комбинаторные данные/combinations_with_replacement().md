#метод #itertools #python #combinations_with_replacement


Функция `combinations_with_replacement()`возвращает итератор, который содержит все сочетания из элементов переданного итерируемого объекта с повторами. Другими словами, один элемент в одном сочетании может встречаться более одного раза. Каждое сочетание заключено в кортеж нужной длины

Аргументы функции:
- `iterable` — итерируемый объект
- `r` — целое число, длина возвращаемых кортежей

```python
from itertools import combinations_with_replacement

numbers = [1, 2, 3, 4]

print(list(combinations_with_replacement(numbers, 1)))
print(list(combinations_with_replacement(numbers, 2)))
print(list(combinations_with_replacement(numbers, 3)))
```
```
[(1,), (2,), (3,), (4,)]
[(1, 1), (1, 2), (1, 3), (1, 4), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (4, 4)]
[(1, 1, 1), (1, 1, 2), (1, 1, 3), (1, 1, 4), (1, 2, 2), (1, 2, 3), (1, 2, 4), (1, 3, 3), (1, 3, 4), (1, 4, 4), (2, 2, 2), (2, 2, 3), (2, 2, 4), (2, 3, 3), (2, 3, 4), (2, 4, 4), (3, 3, 3), (3, 3, 4), (3, 4, 4), (4, 4, 4)]
```
Обратите внимание на то, что сочетания генерируются в порядке сортировки переданного итерируемого объекта

```python
from itertools import combinations_with_replacement

letters = 'bca'

print(list(combinations_with_replacement(letters, 1)))
print(list(combinations_with_replacement(letters, 2)))
```
```
[('b',), ('c',), ('a',)]
[('b', 'b'), ('b', 'c'), ('b', 'a'), ('c', 'c'), ('c', 'a'), ('a', 'a')]
```