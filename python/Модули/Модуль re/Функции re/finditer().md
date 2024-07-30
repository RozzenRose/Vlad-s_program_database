#python #re #finditer #метод


Функция `finditer()` возвращает все неперекрывающиеся совпадения с регулярным выражением в виде итератора, содержащего объекты соответствия (тип `Match`). Строка сканируется слева направо, и совпадения возвращаются в найденном порядке

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `string` — строка для поиска
- `flags=0` — один или несколько флагов (необязательный аргумент)

```python
import re

text = 'ул. Часовая, дом № 25, корпус 2, квартира 69'
result = re.finditer('\d+', text)

print(type(result))
print(list(result))
```
```
<class 'callable_iterator'>
[<re.Match object; span=(19, 21), match='25'>, <re.Match object; span=(30, 31), match='2'>, <re.Match object; span=(42, 44), match='69'>]
```

```python
import re

result = re.finditer('#(\w+)#', '#foo#.#bar#.#baz#')

for match in result:
    print(match)
    print(match.group(), match.group(1))
```
```
<re.Match object; span=(0, 5), match='#foo#'>
#foo# foo
<re.Match object; span=(6, 11), match='#bar#'>
#bar# bar
<re.Match object; span=(12, 17), match='#baz#'>
#baz# baz
```

Функции `findall()` и `finditer()` очень похожи, но есть два отличия:
- функция `findall()` возвращает список, в то время как функция `finditer()` возвращает итератор
- функция `findall()` возвращает список, содержащий фактические строки, в то время как элементами итератора, который возвращает функция `finditer()`, являются объекты соответствия (тип `Match`)