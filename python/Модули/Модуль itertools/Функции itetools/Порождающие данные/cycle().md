#метод #itertools #python #cycle

Функция `cycle()` возвращает итератор циклично генерирующий последовательность элементов переданного итерируемого объекта
Аргументы функции:
- `iterable` — итерируемый объект

Обратите внимание на то, что функция `cycle()` сохраняет копию каждого элемента их `iterable`. Когда итерируемый объект `iterable` исчерпан, функция начинает возвращать элементы их сохраненной копии
```python
from itertools import cycle

for index, char in enumerate(cycle('abcd')):
    if index < 7:
        print(char)
    else:
        break

cycle_iter = cycle([0, 1])
print(next(cycle_iter), next(cycle_iter), next(cycle_iter), next(cycle_iter), next(cycle_iter))

for i in zip(range(7), cycle(['a', 'b', 'c'])):
    print(i)
```
```
a
b
c
d
a
b
c
0 1 0 1 0
(0, 'a')
(1, 'b')
(2, 'c')
(3, 'a')
(4, 'b')
(5, 'c')
(6, 'a')
```
Для выполнения `cycle()` может потребоваться значительное количество памяти в зависимости от длинны `iterable`

Функция `cycle()` примерно эквивалента следующему коду:
```python
def cycle(iterable):
    saved = []
    for element in iterable:
        yield element
        saved.append(element)
    while saved:
        for element in saved:
            yield element
```