#python #match #re #split 


Функция `re.split()`  разбивает строку на подстроки, используя регулярное выражение в качестве разделителя, и возвращает подстроки в виде списка

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `string` — строка для поиска
- `maxsplit=0` — максимальное количество разбиений (необязательный аргумент)
- `flags=0` — один или несколько флагов (необязательный аргумент)

```python
import re

result = re.split(r'[,;.]', 'foo,bar.baz;qux;stepik,beegeek')

print(result)
```
разбивает строку на подстроки, используя в качестве разделителя один из трех символов `,`, `;` или `.`, и выводит:
```
['foo', 'bar', 'baz', 'qux', 'stepik', 'beegeek']
```

```python
import re

result = re.split(r'\s*[,;.]\s*', 'foo,   bar. baz   ;    qux ;  stepik   ,   beegeek')

print(result)
```
разбивает строку на подстроки, используя в качестве разделителя один из трех символов `,`, `;` или `.`, окруженный любым количеством пробелов, и выводит:
```
['foo', 'bar', 'baz', 'qux', 'stepik', 'beegeek']
```

Если шаблон регулярного выражение содержит группы захвата, то возвращаемые список помимо подстрок также включает в себя эти группы:
```python
import re

result1 = re.split(r'\s*([,;.])\s*', 'foo,   bar. baz   ;    qux ;  stepik   ,   beegeek')
result2 = re.split(r'(\s*[,;.]\s*)', 'foo,   bar. baz   ;    qux ;  stepik   ,   beegeek')

print(result1)
print(result2)
```
```
['foo', ',', 'bar', '.', 'baz', ';', 'qux', ';', 'stepik', ',', 'beegeek']
['foo', ',   ', 'bar', '. ', 'baz', '   ;    ', 'qux', ' ;  ', 'stepik', '   ,   ', 'beegeek']
```
Как мы видим, результирующий список `result2` содержит не только подстроки, но и сами группы, соответствующие шаблону регулярного выражения:
- `',   '`
- `'. '`
- `'   ;    '`
- `' ;  '`
- `'   ,   '`

Такое поведение может быть полезно, если мы хотим разделить строку на подстроки некоторыми разделителями, затем обработать все полученные подстроки, а затем снова собрать строку вместе, используя те же разделители
```python
import re

string = 'foo,bar.baz;  qux;stepik,    beegeek'
regex = r'(\s*[,;.]\s*)'

result = re.split(regex, string)

for index, value in enumerate(result):
    if not re.fullmatch(regex, value):
        result[index] = f'[{value}]'

new_string = ''.join(result)

print(string)
print(new_string)
```
```
foo,bar.baz;  qux;stepik,    beegeek
[foo],[bar].[baz];  [qux];[stepik],    [beegeek]
```
Если нам нужно использовать группы, но мы не хотим, чтобы разделители включались в результирующий список, то можно использовать группы без захвата, используя синтаксис `(?:<regex>)`
```python
import re

result = re.split(r'(?:\s*[,;.]\s*)', 'foo,   bar. baz   ;    qux ;  stepik   ,   beegeek')

print(result)
```
```
['foo', 'bar', 'baz', 'qux', 'stepik', 'beegeek']
```


### Ограничение количества разбиений
Мы можем использовать необязательный аргумент `maxsplit` для задания максимального количества разбиений строки
```python
import re

text = 'foo; bar;   baz; qux;   stepik;    beegeek'
regex = r';\s*'

result1 = re.split(regex, text)
result2 = re.split(regex, text, maxsplit=2)
result3 = re.split(regex, text, maxsplit=4)

print(result1)
print(result2)
print(result3)
```
```
['foo', 'bar', 'baz', 'qux', 'stepik', 'beegeek']
['foo', 'bar', 'baz; qux;   stepik;    beegeek']
['foo', 'bar', 'baz', 'qux', 'stepik;    beegeek']
```
Как мы видим, последний элемент в результирующем списку - это часть начальной строки, оставшаяся после того, как произошли все разделения в заданном количестве `maxsplit`