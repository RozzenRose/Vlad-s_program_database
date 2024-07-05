#метод #itertools #python #compress

Функция `compress()` предлагает другой способ фильтрации содержимого итерируемого объекта. Вместо того чтобы вызывать функцию, она использует значения другого итерируемого объекта для индикации того, следует ли принять значение или игнорировать его

Аргументы функции:
- `iterable` — итерируемый объект
- `selectors` — итерируемый объект, состоящий из значений `True, False`, который предоставляет значения, указывающие на то, какие входные значения следует брать, а какие следует игнорировать
```python
from itertools import compress

data = 'ABCDEF'
selectors = [True, False, True, False, True, False]

result = compress(data, selectors)
print(list(result))
```
```
['A', 'C', 'E']
```
Обратите внимание на то, что функция `compress()` останавливается, когда исчерпан любой из итерируемых объектов `iterable` или `selectors`
```python
from itertools import compress

data = 'ABCDEF'
selectors = [True, False, True]

result = compress(data, selectors)
print(list(result))
```
```
['A', 'C']
```
Функция `compress()` примерно эквивалентна следующему коду:
```python
def compress(iterable, selectors):
    for value, selector in zip(iterable, selectors):
        if selector:
            yield value
```