#python #start #end #re #метод 


Методы `start()` и `end()` возвращают индексы начала и конца подстроки, которая совпала с регулярным выражением
```python
from re import search

match = search('\d+', 'foo123bar456baz')

print(match)
print(match.start())
print(match.end())
```
```
<re.Match object; span=(3, 6), match='123'>
3
6
```

Значения, возвращаемые методами `start()` и `end()`, можно использовать для того чтобы получить подстроку, которая совпала с шаблоном регулярного выражения.
```python
from re import search

text = 'foo123bar456baz'

match = search('\d+', text)

start = match.start()
end = match.end()

print(match)
print(text[start:end])
print(match.group())           # аналогично предыдущей строке
```
```
<re.Match object; span=(3, 6), match='123'>
123
123
```

В методы `start()` и `end()` также можно передать номер или названия группы. В этом случае методы вернут индексы начала и конца подстроки, совпадающей с нужной группой
```python
from re import search

text = 'foo123bar456baz'

match = search('(\d+)\D+(?P<num>\d+)', text)

print(match)
print(match.group(), match.start(), match.end())
print(match.group(1), match.start(1), match.end(1))
print(match.group('num'), match.start('num'), match.end('num'))
```
```
<re.Match object; span=(3, 12), match='123bar456'>
123bar456 3 12
123 3 6
456 9 12
```

Если некоторая группа соответствует строке нулевой длины, то значения возвращаемые методами `start()` и `end()` будут равны. Достаточно разумное поведение, учитывая, что значения, возвращаемые методами `start()` и `end()` действуют как индексы среза. Любой срез строки, в котором начальный и конечный индексы равны, всегда будет пустой строкой
```python
from re import search

match = search('foo(\d*)bar', 'foobar')

print(match)
print(match.group())
print(match.start(), match.end())
print(match.start(1), match.end(1))
```
```
<re.Match object; span=(0, 6), match='foobar'>
foobar
0 6
3 3
```

Особый случай возникает, когда регулярное выражение содержит группу, не участвующую в сопоставлении. В этом случае оба метода вернут значение `-1`
```python
from re import search

match = search('(\w+),(\w+),(\w+)?', 'foo,bar,')

print(match.group(3))
print(match.start(3), match.end(3))
```
```
None
-1 -1
```