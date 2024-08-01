#python #subn #re #метод 


Функция `subn()` идентична функции `sub()`, за тем исключением, что она возвращает кортеж, состоящий из измененной строки и количества сделанных замен

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `repl` — строка замены
- `string` — строка для поиска
- `count=0` — максимальное число замен (необязательный аргумент)
- `flags=0` — один или несколько флагов (необязательный аргумент)
```python
import re

text = 'foo.123.bar.456.baz.789.geek'

result1 = re.subn(r'\d+', r'#', text)
result2 = re.subn(r'[a-z]+', r'(*)', text, count=2)

print(result1)
print(result2)
```
```
('foo.#.bar.#.baz.#.geek', 3)
('(*).123.(*).456.baz.789.geek', 2)
```

```python
import re
def func(match_obj):
    s = match_obj.group(0)         # строка совпадения
    if s.isdigit():
        return str(int(s) * 10)
    else:
        return s.upper()

result = re.subn(r'\w+', func, 'foo.10.bar.20.baz30.40')

print(result)
```
```
('FOO.100.BAR.200.BAZ30.400', 6)
```