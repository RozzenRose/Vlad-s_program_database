#python #groups #re #метод


Метод `groups()` возвращает кортеж, содержащий все захваченные группы
```python
from re import search

match = search('(\w+),(\w+),(\w+)?', 'foo,bar,')

print(match.groups())
```
```
('foo', 'bar', None)
```

Группы, которые не смогли захватить какой-либо результат, по умолчанию будут иметь значение `None`. Если в такой ситуации требуется вернуть значение, отличное от `None`, то используется необязательный аргумент `default`

```python
from re import search

match = search('(\w+),(\w+),(\w+)?', 'foo,bar,')

print(match.groups(-1))                     # позиционный аргумент
print(match.groups(''))
print(match.groups(default='----'))         # именованный аргумент
print(match.groups(default=False))
```
```
('foo', 'bar', -1)
('foo', 'bar', '')
('foo', 'bar', '----')
('foo', 'bar', False)
```