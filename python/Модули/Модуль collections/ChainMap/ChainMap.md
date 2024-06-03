#collections #Python #ChainMap #объект


Тип данных `ChainMap`, представляет из себя объединение нескольких словарей. Этот объект группирует несколько словарей вместе, что позволяет рассматривать их как единое целое.

### [[Методы ChainMap]]
### Создание `ChainMap` объектов:
Объекты `ChainMap` обычно создаются на основе словарей `dict`:
```python
from collections import ChainMap

empty_chain_map = ChainMap()                      # создаем пустой ChainMap объект
print(empty_chain_map)

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

chain_map = ChainMap(numbers, letters)            # создаем ChainMap объект на основе словарей numbers и letters
print(chain_map)
```
```
ChainMap({})
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```
 Так же можно создавать объекты типа `ChainMap`, используя метод `fromkeys()`.
 ```python
from collections import ChainMap

chain_map1 = ChainMap.fromkeys(['one', 'two', 'three'])
chain_map2 = ChainMap.fromkeys(['one', 'two', 'three'], -1)

print(chain_map1)
print(chain_map2)
```
```
ChainMap({'one': None, 'two': None, 'three': None})
ChainMap({'one': -1, 'two': -1, 'three': -1})
```


### Доступ к элементам `ChainMap` объекта:
Для получения по ключу в `ChainMap` объектах используется такой же механизм, как и в обычных словарях `dict`. Либо мы используем квадратные скобки, либо метод `get()`
```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}
alpha_num = ChainMap(numbers, letters)

print(alpha_num['one'])
print(alpha_num['b'])
print(alpha_num.get('a'))
print(alpha_num.get('c'))
print(alpha_num.get('d', False))
```
```
1
B
A
None
False
```
Рассмотрим `ChainMap` объект, в котором есть повторяющиеся ключи в объединяемых словарях.
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets['dogs'])
print(pets['cats'])

print(pets['pythons'])
print(pets['tigers'])
```
```
15
8
9
3
```
Как мы видим, в ситуации, когда у объединяемых словарей есть повторяющиеся ключи, мы получаем только значение из первого словаря, в котором встречается этот ключ. Таким образом, поиcк по `ChainMap` объекту всегда осуществляется в том же порядке, в котором словари были указаны при создании `ChainMap` объекта, при этом поиск останавливается, как только значение по нужному ключу найдено.

### Итерирование по `ChainMap` объекту
Итерирование во `ChainMap` объекту происходит в обратном порядке от последнего словаря к первому.
```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)

for key in alpha_num:
    print(key, '->', alpha_num[key])
```
```
a -> A
b -> B
one -> 1
two -> 2
```
При этом если в `ChainMap` объекте есть повторяющиеся ключи в объединяемых словарях, то мы будем получать первое из значений.
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets:
    print(key, '->', pets[key])
```
```
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9
```
При итерировании по `ChainMap` объекту мы можем использовать методы `keys()`, `values()`, `items()`
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for key in pets.keys():
    print(key, '->', pets[key])

print()

for value in pets.values():
    print(value)
    

print()

for key, value in pets.items():
    print(key, '->', value)
```
```
dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9

15
8
3
9

dogs -> 15
cats -> 8
tigers -> 3
pythons -> 9
```
### Изменение данных в `ChainMap` объекте
Для изменения объектов типа `ChainMap` мы можем использовать те же способы, что и для изменения обычного словаря `dict`. Мы можем обновлять, добавлять, удалять и извлекать элементы из `ChainMap` объекта. При этом нужно знать, что все эти операции действуют только на первый из объединяемых словарей.
```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap(numbers, letters)
print(alpha_num)

alpha_num['c'] = 'C'
print(alpha_num)

alpha_num['b'] = 'b'
print(alpha_num)

alpha_num.pop('two')
print(alpha_num)

del alpha_num['c']
print(alpha_num)

alpha_num.clear()
print(alpha_num)
```
```
ChainMap({'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'two': 2, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'c': 'C', 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({'one': 1, 'b': 'b'}, {'a': 'A', 'b': 'B'})
ChainMap({}, {'a': 'A', 'b': 'B'})
```
При попытке удаления значения по ключу, которого нет в первом словаре, возникает ошибка `KeyError`, даже если указанный ключ есть в одном из объединяемых словарей, кроме первого.

Поскольку все изменения `ChainMap` объекта действуют только на первый из объединяемых словарей.
```python
from collections import ChainMap

numbers = {'one': 1, 'two': 2}
letters = {'a': 'A', 'b': 'B'}

alpha_num = ChainMap({}, numbers, letters)
print(alpha_num)

alpha_num['colon'] = ':'
alpha_num['comma'] = ','
alpha_num['period'] = '.'
print(alpha_num)
```
```
ChainMap({}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
ChainMap({'colon': ':', 'comma': ',', 'period': '.'}, {'one': 1, 'two': 2}, {'a': 'A', 'b': 'B'})
```
Таким образом, указывая в качестве первого аргумента для `ChainMap` пустой словарь, мы получаем поведение, при котором все изменения `ChainMap` объекта не затрагивают объединяемые словари.