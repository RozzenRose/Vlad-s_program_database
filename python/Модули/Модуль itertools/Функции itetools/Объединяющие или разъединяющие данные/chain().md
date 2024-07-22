#метод #itertools #python #chain


Функция `chain()` возвращает итератор, который последовательно генерирует элементы всех переданных итерируемых объектов

Аргументы функции:
- `*iterables` — итерируемые объекты

```python
from itertools import chain

chain_iter1 = chain('ABC', 'DEF')
print(*chain_iter1)

chain_iter2 = chain(enumerate('ABC'))
print(*chain_iter2)

for i in chain([1, 2, 3], ['a', 'b', 'c'], ('Timur', 29, 'Male', 'Teacher')):
    print(i, end=' ')
```
```
A B C D E F
(0, 'A') (1, 'B') (2, 'C')
1 2 3 a b c Timur 29 Male Teacher 
```

Функция `chain()` упрощает обработку нескольких итерируемых объектов, не требуя предварительного конструирования объединенного списка

Функция `chain()` примерно эквивалентна следующему коду:
```python
def chain(*iterables):
    for it in iterables:
        for element in it:
            yield element
```