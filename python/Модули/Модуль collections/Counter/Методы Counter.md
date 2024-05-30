#collections #Python #Counter #объект #most_common #elements #total #subtract


#### Метод `most_common()`:
Метод `most_common()` возвращает список наиболее повторяемых элементов и количество каждого из них. Метод возвращает список кортежей вида `(ключ, число вхождений)`
```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common())
print(numbers.most_common())
```
```
[('i', 4), ('s', 4), ('p', 2), ('m', 1)]
[(5, 3), (7, 3), (9, 3), (1, 2), (6, 1), (3, 1), (2, 1)]
```
Если методу `most_common()` передать целочисленный аргумент `n`, то он вернет `n` самых часто повторяющихся элементов.
```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common(3))
print(numbers.most_common(4))
```
```
[('i', 4), ('s', 4), ('p', 2)]
[(5, 3), (7, 3), (9, 3), (1, 2)]
```
Для поиска самых редких элементов, можно использовать срезы с отрицательным шагом.
```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(letters.most_common()[-1])
print(letters.most_common()[::-1])
print(numbers.most_common()[-3:-1])
```
```
('m', 1)
[('m', 1), ('p', 2), ('s', 4), ('i', 4)]
[(6, 1), (3, 1)]
```
Для корректной работы метода `most_common()` нужно, чтобы значения в `Counter` словаре поддерживали сортировку.

#### Метод `elements()`:
Метод `elements()` возвращает итератор по элементам, в котором каждый элемент повторяется столько раз, во сколько установлено его значение. Элементы возвращаются в порядке их появления.
```python
from collections import Counter

letters = Counter('mississippi')
numbers = Counter([5, 6, 7, 1, 3, 9, 9, 1, 2, 5, 5, 7, 7, 9])

print(list(letters.elements()))
print(list(numbers.elements()))
```
```
['m', 'i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p']
[5, 5, 5, 6, 7, 7, 7, 1, 1, 3, 9, 9, 9, 2]
```
Если значение в `Counter` ниже единицы, метод `elements()` проигнорирует такой ключ
```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(list(letters.elements()))
```
```
['i', 'i', 'i', 'i', 's', 's', 's', 's', 'p', 'p', 'm']
```

#### Метод `total()`:
Этот метод вычисляет сумму всех значений `Counter` словаря, включая отрицательные.
```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(letters.total())
```
```
-87
```
В более ранних версиях Python приведенный выше код можно заменить кодом:
```python
from collections import Counter

letters = Counter(i=4, s=4, a=0, p=2, b=-98, m=1)

print(sum(letters.values()))
```

#### Метод `subtract()`:
Вычитает из значений элементов одного экземпляра `Counter` значения элементов другого словаря. Метод `subtract()` подобен методу `update()`, но вычитает количества, а не складывает их. При этом у результирующего словаря значения ключей  могут быть нулевыми или отрицательными.
```python
from collections import Counter

counter1 = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
counter2 = Counter(i=2, s=20, a=6, p=12, m=1, z=69)

counter1.subtract(counter2)       # обновляем значения в counter1

print(counter1)
```
```
Counter({'b': 98, 's': 20, 'p': 8, 'i': 2, 'z': 0, 'm': -1, 'a': -5})
```
Мы можем использовать метод `subttract()` с именованными аргументами. Вызов метода `counter1.subtract(i=2, s=20, p=6, p=12, m=1, z=69)` равнозначен вызову метода `counter1.subtract(counter2)`
```python
from collections import Counter

counter = Counter(i=4, s=40, a=1, p=20, b=98, z=69)
letters = 'iisssssapppz'
counter.subtract(letters)       # обновляем значения в counter

print(counter)
```
```
Counter({'b': 98, 'z': 68, 's': 35, 'p': 17, 'i': 2, 'a': 0})
```

#### Операторы `+, -, &, |`:
Такие методы как `update()`, `subtract()` объединяют словари путем сложения и вычитания количества соответствующих элементов. Python предоставляет удобные операторы сложения (`+`) и вычитания (`-`), которые могут заменить вызовы данных методов.
```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 + counter2)
print(counter1 - counter2)
print(counter2 - counter1)
```
```
Counter({'s': 48, 'p': 20, 'i': 12, 'm': 4})
Counter({'s': 32, 'i': 8})
Counter({'m': 2})
```
При использовании `+` и `-`  из результирующего словаря исключаются элементы с нулевыми и отрицательными значениями. 

Так же в Python есть операторы `&` и `|`, которые возвращают минимум и максимум из соответствующих значений.
```python
from collections import Counter

counter1 = Counter(i=10, s=40, p=10, m=1)
counter2 = Counter(i=2, s=8, p=10, m=3)

print(counter1 & counter2)
print(counter1 | counter2)
```
```
Counter({'p': 10, 's': 8, 'i': 2, 'm': 1})
Counter({'s': 40, 'i': 10, 'p': 10, 'm': 3})
```
