#python #re #escape #метод


Функция `escape()` выполняет экранирование специальных символов в строке. Это полезно в ситуациях, когда регулярное выражение представляет из себя простую строку, которая может содержать метасимволы

Аргументы функции:
- `pattern` — шаблон регулярного выражения

```python
from re import escape

print(escape('http://www.stepik.org'))
```
```
http://www\.stepik\.org
```
Функция `escape()` выполнила экранирование символа точки

```python
from re import escape

operators = ['+', '-', '*', '/', '**']
print(','.join(map(escape, operators)))
```
```
\+,\-,\*,/,\*\*
```
Функция `escape()` выполнила экранирование всех арифметических оператором, кроме `/`