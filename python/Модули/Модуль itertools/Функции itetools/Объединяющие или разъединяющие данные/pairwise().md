#метод #itertools #python #pairwise


Функция `pairise()` возвращает итератор, содержащий последовательные перекрывающиеся пары в виде кортежей, взятые их исходного итерируемого объекта

Аргументы функции:
- `iterable` — итерируемый объект

```python
from itertools import pairwise

print(*pairwise('ABCDEFG'))
print(*pairwise([1, 2, 3, 4, 5]))
```
```
('A', 'B') ('B', 'C') ('C', 'D') ('D', 'E') ('E', 'F') ('F', 'G')
(1, 2) (2, 3) (3, 4) (4, 5)
```

Функция `pairwise()` примерно эквивалентна следующему коду:
```python
def pairwise(iterable):
    a, b = tee(iterable)
    next(b, None)
    return zip(a, b)
```