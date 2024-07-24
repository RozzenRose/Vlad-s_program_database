#метод #itertools #python #product 


Функция `product()` возвращает итератор, который содержит декартово произведение всех переданных итерируемых объектов

Аргументы функции:
- `*iterables` — итерируемые объекты
- `repeat` — целое число, количество повторов; по умолчанию имеет значение 1

```python
from itertools import product

numbers = [1, 2]
letters = ['x', 'y', 'z']
flags = [False, True]

print(list(product(numbers, letters)))
print(list(product(letters, numbers)))
print(list(product(letters, numbers, flags)))
```
```
[(1, 'x'), (1, 'y'), (1, 'z'), (2, 'x'), (2, 'y'), (2, 'z')]
[('x', 1), ('x', 2), ('y', 1), ('y', 2), ('z', 1), ('z', 2)]
[('x', 1, False), ('x', 1, True), ('x', 2, False), ('x', 2, True), ('y', 1, False), ('y', 1, True), ('y', 2, False), ('y', 2, True), ('z', 1, False), ('z', 1, True), ('z', 2, False), ('z', 2, True)]
```
Чтобы вычислить декартово произведение итерируемого объекта с самим собой, можно использовать аргумент `repeat`
```python
from itertools import product

letters = 'abc'

print(list(product(letters, repeat=2)))
```
```
[('a', 'a'), ('a', 'b'), ('a', 'c'), ('b', 'a'), ('b', 'b'), ('b', 'c'), ('c', 'a'), ('c', 'b'), ('c', 'c')]
```
строка `product(letters, letters)` делает то же самое