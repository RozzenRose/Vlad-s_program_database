#python #span #re #метод 


Метод `span()` возвращает индексы начала и конца подстроки в виде кортежа, совпала с регулярным выражением. В метод `span()` также можно передать номер или название группы. В этом случае метод вернет индексы начала и конца подстроки в виде кортежа, совпадающей с нужной группой
```python
from re import search

match = search('(\d+)\D+(?P<num>\d+)', 'foo123bar456baz')

print(match)
print(match.span())
print(match.span(1))
print(match.span('num'))
```
```
<re.Match object; span=(3, 12), match='123bar456'>
(3, 12)
(3, 6)
(9, 12)
```