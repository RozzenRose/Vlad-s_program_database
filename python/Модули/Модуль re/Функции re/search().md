#python #re #search #метод


Функция `search()` ищет первое совпадение в строке регулярного выражения и возвращает специальный объект соответствия (тип `Match`) или значение `None`, если ни одна позиция в строке не соответствует регулярному выражению

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `string` — строка для поиска
- `flags=0` — один или несколько флагов (необязательный аргумент)

```python
from re import search

match1 = search('super', 'superstition')
match2 = search('super', 'insuperable')
match3 = search('super', 'without')

print(match1)
print(match2)
print(match3)
```
```
<re.Match object; span=(0, 5), match='super'>
<re.Match object; span=(2, 7), match='super'>
None
```

```python
from re import search

match1 = search('\d+', 'foo123bar')
match2 = search('[a-z]+', '123FOO456')
match3 = search('\d+', 'foo.bar')

print(match1)
print(match2)
print(match3)
```
```
<re.Match object; span=(3, 6), match='123'>
None
None
```
