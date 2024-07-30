#python #match #re #метод


Функция `match()` возвращает специальный объект соответствия (тип `Match`), если **начало строки** соответствует регулярному выражению или значение `None` в противном случае

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `string` — строка для поиска
- `flags=0` — один или несколько флагов (необязательный аргумент)

```python
from re import match

match1 = match('super', 'superstition')
match2 = match('super', 'insuperable')

print(match1)
print(match2)
```
```
<re.Match object; span=(0, 5), match='super'>
None
```

```python
from re import search, match

match1 = search('\d+', '123foobar')
match2 = search('\d+', 'foo123bar')
match3 = match('\d+', '123foobar')
match4 = match('\d+', 'foo123bar')

print(match1)
print(match2)
print(match3)
print(match4)
```
```
<re.Match object; span=(0, 3), match='123'>
<re.Match object; span=(3, 6), match='123'>
<re.Match object; span=(0, 3), match='123'>
None
```

Функция `search()` находит соответствие, когда последовательность цифр находится как в начале строки, так и в середине. При этом функция `match()` находит соответствие только тогда, когда последовательность цифр находится вначале

