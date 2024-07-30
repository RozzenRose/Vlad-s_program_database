#python #group #re #метод


Метод `group()` возвращает одну или несколько подгрупп совпадения. Если метод вызывается без аргументов, то возвращается вся подстрока, которая совпала с шаблоном регулярного выражения
```python
from re import search

match = search('(\w+),(\w+),(\w+)', 'foo,bar,baz')

print(match.group())                       # вся строка
print(match.group(0))                      # вся строка
print(match.group(1))                      # подгруппа
print(match.group(2))                      # подгруппа
print(match.group(3))                      # подгруппа
print(match.group(1, 2, 3))                # кортеж
```
```
foo,bar,baz
foo,bar,baz
foo
bar
baz
('foo', 'bar', 'baz')
```
В качестве аргумента можно указать как одну группу, так и несколько. В первом случае метод вернет строку, соответствующую группе, во втором - кортежи строк, соответствующих указанным группам

Вызов без аргументов равнозначен вызову с аргументом `0`

Если методу `group()` передать индекс несуществующей группы, то будет возбуждено исключение
```python
from re import search

match = search('(\w+),(\w+),(\w+)', 'foo,bar,baz')

print(match.group(4))
```
```
IndexError: no such group
```

Переданная в качестве аргумента группа может появляться несколько раз, при этом мы можем указать любые группы в любом порядке
```python
from re import search

match = search('(\w+),(\w+),(\w+)', 'foo,bar,baz')

print(match.group(1, 2, 3, 1, 2, 2, 3, 3, 3, 3))
```
```
('foo', 'bar', 'baz', 'foo', 'bar', 'bar', 'baz', 'baz', 'baz', 'baz')
```
Пронумерованные группы отсчитываются от единицы

Если мы пользуемся **именованными группами**, используя синтаксис `(?P<name><regex>)`, тогда мы можем использовать название группы в качестве аргумента метода `group()`
```python
from re import search

match = search('(?P<w1>\w+),(?P<w2>\w+),(?P<w3>\w+)', 'foo,bar,baz')

print(match.group())
print(match.group('w1'))
print(match.group('w2'))
print(match.group('w3'))
print(match.group('w1', 'w2', 'w3', 'w2', 'w3'))
```
```
foo,bar,baz
foo
bar
baz
('foo', 'bar', 'baz', 'bar', 'baz')
```

Метод `group()` может возвращать в качестве группы значение `None`. Так происходит в ситуации, когда группа не участвует в сопоставлении
```python
from re import search

match = search('(\w+),(\w+),(\w+)?', 'foo,bar,')

print(match.group())
print(match.group(0))
print(match.group(1))
print(match.group(2))
print(match.group(3))
print(match.group(1, 2, 3))
```
```
foo,bar,
foo,bar,
foo
bar
None
('foo', 'bar', None)
```