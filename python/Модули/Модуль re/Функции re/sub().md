#python #sub #re #метод 


Функция `sub()` возвращает строку, полученную путем замены всех найденных неперекрывающихся вхождений регулярного выражения `pattern` в строку `string` на строку замены `repl`

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `repl` — строка замены
- `string` — строка для поиска
- `count=0` — максимальное число замен (необязательный аргумент)
- `flags=0` — один или несколько флагов (необязательный аргумент)
В случае, если шаблон не будет найден в предложении, строка вернется без изменений

Аргумент `repl` может быть строкой или функцией. Если `repl` это строка, то в ней обрабатываются все обратные слеши, то есть `\n` преобразуется в символ новой строки, `\r` преобразуется в возврат каретки и т.д.
```python
import re

text = 'Java самый популярный язык программирования в 2022 году.'

res = re.sub(r'Java', r'Python', text)

print(res)
```
```
Python самый популярный язык программирования в 2022 году.
```
Важно отметить, что `sub()` создает новую строку, а не модифицирует старую.

### Замена строкой
Если аргумент `repl` является строкой, то функция `sub()` вставляет ее в строку поиска `string` вместо любых последовательностей, соответствующих регулярному выражению `pattern`
```python
import re

text = 'foo.123.bar.456.baz.789.geek'

result1 = re.sub(r'\d+', r'#', text)
result2 = re.sub(r'[a-z]+', r'(*)', text)

print(result1)
print(result2)
```
```
foo.#.bar.#.baz.#.geek
(*).123.(*).456.(*).789.(*)
```
Первый вызов функции `sub()` заменяет последовательность подряд идущих цифр на символ `#`. Второй вызов функции `sub()` заменяет последовательность подряд идущих строчных латинских букв на `(*)`

При использовании функции `sub()` мы также можем использовать пронумерованные обратные ссылки (`\<n>`) в аргументе `repl`, которым будет соответствовать текст захваченной группы. Обратные ссылки, такие как `\2`, заменяются подстрокой, соответствующей группе №2 в шаблоне регулярного выражения
```python
import re

result = re.sub(r'(\w+),bar,baz,(\w+)', r'\2,bar,baz,\1', r'foo,bar,baz,qux')

print(result)
```
```
qux,bar,baz,foo
```
Захваченные группы `w1` и `w2` содержат `bar` и `baz`. В строке замены `foo\g<w2>,\g<w1>,qux`, `bar` заменяет `\g<w1>`, а `baz` заменяет `\g<w2>`


### Замена с помощью функции 
Если в качестве аргумента `repl` использовать функцию, то функция `sub()` вызовет эту функцию для каждого найденного совпадения. Она передает каждый соответствующий объект совпадения (тип `Match` ) в качестве аргумента функции для предоставления информации о совпадении, при этом возвращаемое их функции значение становится строкой замены
```python
import re
def func(match_obj):
    s = match_obj.group(0)         # строка совпадения
    if s.isdigit():
        return str(int(s) * 10)
    else:
        return s.upper()

result = re.sub(r'\w+', func, r'foo.10.bar.20.baz30.40')

print(result)
```
```
FOO.100.BAR.200.BAZ30.400
```
В этом примере функция `func()` вызывается для каждого найденного совпадения. Таким образом, функция `sub()` преобразует каждую буквенно-цифровую часть строки в верхний регистр и умножает каждую числовую часть на `10`

Функция, передаваемая в качестве аргумента `repl`, должна принимать один аргумент - объект соответствия (тип `Match`) и возвращать строку замены

### Ограничение количества замен
Мы можем использовать необязательные аргументы `count` (количество замен) и `flags` для более детальной настройки замены
```python
import re

text = 'Java самый популярный язык программирования в 2022 году. Язык java — строго типизированный объектно-ориентированный язык программирования общего назначения, разработанный компанией Sun Microsystems. Приложения Java обычно транслируются в специальный байт-код, поэтому они могут работать на любой компьютерной архитектуре, для которой существует реализация виртуальной Java-машины.'

res = re.sub(r'Java', r'Python', text, count=3, flags=re.I)

print(res)
```
```
Python самый популярный язык программирования в 2022 году. Язык Python — строго типизированный объектно-ориентированный язык программирования общего назначения, разработанный компанией Sun Microsystems. Приложения Python обычно транслируются в специальный байт-код, поэтому они могут работать на любой компьютерной архитектуре, для которой существует реализация виртуальной Java-машины.
```
