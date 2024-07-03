#метод #itertools #python #repeat 

Функция `repeat()` возвращает итератор. бесконечного генерирующий единственное значение, переданное в качестве аргумента. Количество генераций можно ограничить с помощью необязательного аргумента `times`
Аргументы функции:
- `obj` — любой Python объект
- `times` — количество повторений, по умолчанию имеет значение `None`

```python
from itertools import repeat

for i in repeat('bee-and-geek', 5):
    print(i)

repeat_iter = repeat([1, 2, 3])

print(next(repeat_iter))
print(next(repeat_iter))
print(next(repeat_iter))
```
```
bee-and-geek
bee-and-geek
bee-and-geek
bee-and-geek
bee-and-geek
[1, 2, 3]
[1, 2, 3]
[1, 2, 3]
```
Функция `repeat()` является ленивой, она использует только память, необходимую для хранения одного элемента

`repeat()` удобно использовать совместно с функциями `zip()` и `map()`, если со значениями,  генерируемыми другими итераторами, должно сочетаться некое постоянное значение

Приведенный ниже код объединяет значения `0, 1, 2, 3, 4, ...` со строкой `bee-and-geek`, возвращаемой функцией `repeat()`:
```python
from itertools import count, repeat

for i, s in zip(count(), repeat('bee-and-geek', 5)):
    print(i, s)
```
```
0 bee-and-geek
1 bee-and-geek
2 bee-and-geek
3 bee-and-geek
4 bee-and-geek
```
Приведенный ниже код использует встроенную функцию `map()` для умножения на `2`чисел в диапазоне от `0` до `5`
```python
from itertools import repeat

for a, b, c in map(lambda x, у: (x, у, x * у), repeat(2), range(6)):
    print(f'{a} * {b} = {c}')
```
```
2 * 0 = 0
2 * 1 = 2
2 * 2 = 4
2 * 3 = 6
2 * 4 = 8
2 * 5 = 10
```
В данном случае итератор, возвращаемый функцией `repeat()`, не нуждается в явном ограничении числа генераций, поскольку обработка с помощью функции `map()` прекращается сразу же, как только исчерпывается любой из ее входных итерируемых объектов, а не функция `range()` возвращает только шесть элементов

Функция `repeat()` примерно эквивалентна следующему коду:
```python
def repeat(object, times=None):
    if times is None:
        while True:
            yield object
    else:
        for i in range(times):
            yield object
```