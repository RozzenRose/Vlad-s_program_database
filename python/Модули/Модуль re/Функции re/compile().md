#python #re #compile #метод


Модуль `re` поддерживает возможность предварительной компиляции регулярного выражения в специальный объект, который можно повторно использовать позже. Для этого используется функция `compile()`

Аргументы функции:
- `regex` — шаблон регулярного выражения
- `flags=0` — один или несколько флагов (необязательный аргумент)

Существует два способа использования скомпилированного объекта регулярного выражения

**1 способ**: мы можем его указать в качестве первого аргумента для функции модуля `re`, вместо шаблона регулярного выражения
```python
import re

regex_obj = re.compile('\d+')
text = 'ул. Часовая, дом № 25, корпус 2, квартира 69'
result = re.findall(regex_obj, text)

print(result)
```
```
['25', '2', '69']
```
 В общем случае:
```python
import re

regex_obj = re.compile(<regex>, <flags>)
result = re.search(regex_obj, <string>)     # match(), fullmatch(), findall(), finditer()
```

**2 способ**: мы можем вызывать функции как методы непосредственно из объекта регулярного выражения
```python
import re

regex_obj = re.compile('\d+')
text = 'ул. Часовая, дом № 25, корпус 2, квартира 69'
result = regex_obj.findall(text)

print(result)
```
```
['25', '2', '69']
```
 В общем случае:
```python
import re

regex_obj = re.compile(<regex>, <flags>)
result = regex_obj.search(<string>)         # match(), fullmatch(), findall(), finditer()
```

Приведем пример с использованием флага.
```python
import re

regex_obj = re.compile('ba[rz]', flags=re.I)

result1 = re.search('ba[rz]', 'FOOBARBAZ', flags=re.I)
result2 = re.search(regex_obj, 'FOOBARBAZ')
result3 = regex_obj.search('FOOBARBAZ')

print(result1)
print(result2)
print(result3)
```
```
<re.Match object; span=(3, 6), match='BAR'>
<re.Match object; span=(3, 6), match='BAR'>
<re.Match object; span=(3, 6), match='BAR'>
```
Все три объекта `result1`, `result2`  и `result3` содержит одинаковые значения

Возникает резонный вопрос: для чего нужна предварительная компиляция? Если мы часто используем одно и то же регулярное выражение, то предварительная компиляция позволяет нам отделить определение регулярного выражения от его использования, что повышает читабельность кода
```python
import re

s1, s2, s3, s4 = 'foo.bar', 'foo123bar', 'baz99', 'qux & grault'

print(re.search('\d+', s1))
print(re.search('\d+', s2))
print(re.search('\d+', s3))
print(re.search('\d+', s4))
```
Регулярное выражение `\d+` появляется несколько раз. Если в результате поддержки (изменения) кода, мы решим, что нам нужно другое регулярное выражение, то нам нужно будет изменить его в каждом месте, что не очень удобно.

Приведенный ниже код, решает эту проблему, делая программный код более удобным в сопровождении:
```python
import re

s1, s2, s3, s4 = 'foo.bar', 'foo123bar', 'baz99', 'qux & grault'

regex_obj = re.compile('\d+')

print(regex_obj.search(s1))
print(regex_obj.search(s2))
print(regex_obj.search(s3))
print(regex_obj.search(s4))
```

### Методы скомпилированного объекта регулярного выражения
Скомпилированный объект регулярного выражения поддерживает следующие методы:
- `search(string, pos, endpos)`
- `match(string, pos, endpos)`
- `fullmatch(string, pos, endpos)`
- `findall(string, pos, endpos)`
- `finditer(string, pos, endpos)`

Данные методы ведут себя так же, как соответствующие (одноименные) им функции модуля `re`, за исключением того, что они также поддерживают необязательные аргументы `pos` и `endpos`.

Если аргументы `pos` и `endpos` переданы, то поиск применяется только к части строки `string`, от `pos` (включительно), до `endpos` (не включительно), подобно индексам в срезах.
```python
import re

regex_obj = re.compile('\d+')
text = 'foo12345barbaz'

print(regex_obj.search(text))
print(regex_obj.search(text, pos=4))
print(regex_obj.search(text, endpos=7))
print(regex_obj.search(text, pos=4, endpos=7))
```
```
<re.Match object; span=(3, 8), match='12345'>
<re.Match object; span=(4, 8), match='2345'>
<re.Match object; span=(3, 7), match='1234'>
<re.Match object; span=(4, 7), match='234'>
```