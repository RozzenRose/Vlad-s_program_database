#python #groupdict #re #метод


Метод `groupdict()` возвращает словарь, содержащий все захваченные именованные группы
```python
from re import search

match = search('(?P<w1>\w+),(?P<w2>\w+),(?P<w3>\w+)', 'foo,bar,baz')

print(match.groupdict())
```
```
{'w1': 'foo', 'w2': 'bar', 'w3': 'baz'}
```

Метод `groupdict()`, как и метод `groups()`, принимает необязательный аргумент `default`, который используется для указания значений групп, которые не смогли захватить какой либо результат. По умолчанию значение данного аргумента равно `None`
```python
from re import search

match = search('(?P<w1>\w+),(?P<w2>\w+),(?P<w3>\w+)?', 'foo,bar,')

print(match.groupdict())
print(match.groupdict(default=''))
print(match.groupdict(default='----'))
```
```
{'w1': 'foo', 'w2': 'bar', 'w3': None}
{'w1': 'foo', 'w2': 'bar', 'w3': ''}
{'w1': 'foo', 'w2': 'bar', 'w3': '----'}
```

Если именованных групп в исходном регулярном выражении нет, метод `groupdict()` возвращает пустой словарь
```python
from re import search

match = search('(\w+),(\w+),(\w+)?', 'foo,bar,buz')

print(match.groupdict())
print(match.groupdict(default=''))
print(match.groupdict(default='----'))
```
```
{}
{}
{}
```