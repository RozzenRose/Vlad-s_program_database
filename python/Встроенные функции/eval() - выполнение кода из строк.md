#встроенная_функция #python #метод #eval

Метод `eval()` выполняется строку-выражение, переданную ей в качестве обязательного аргумента`expression`, и возвращает результат выполнения этой строки
```python
eval(expression)
```

Для выполнения строки-выражения, функция `eval()` совершает следующие шаги:
1.  Парсит (**parse**) выражение
2. Компилирует (**compile**) выражение в байт-код
3. Вычисляет (**evaluate**) значение выражения
4. Возвращает (**return**) результат вычисления

Примеры:
```python
expression = '7 + 10'

result = eval(expression)

print(type(result))
print(result)
```
```
<class 'int'>
17
```
Выражения, передаваемые в качестве аргумента функции `eval()`, имеют доступ ко всем встроенным функциям Python
```python
expression1 = "print('Привет из функции eval()')"
expression2 = "len([1, 1, 1, 1, 1])"

result1 = eval(expression1)
result2 = eval(expression2)

print(result1)
print(result2)
```
```no-highlight
Привет из функции eval()
None
5
```
Выражения, передаваемые в качестве аргумента функции `eval()`, имеют доступ ко всем локальным и глобальным переменным.
```python
from math import pi

n = 10
nums = [1, 2, 3, 4, 5]

expression1 = "n + 7"
expression2 = "[i**2 for i in nums]"
expression3 = "pi / 2"

result1 = eval(expression1)
result2 = eval(expression2)
result3 = eval(expression3)

print(result1)
print(result2)
print(result3)
```
```
17
[1, 4, 9, 16, 25]
1.5707963267948966
```
Важно отметить, что не все языковые конструкции являются выражениями (expression). Операторами, которые нельзя использовать в качестве выражений, являются, например: `while, for, if, def, import, class, raise` и т.д.
```python
num = 17

eval('if num == 10: print(num)')
```
```
SyntaxError: invalid syntax
```
Если ключевое слово `for` используется в списочном выражении, то функция `eval()` может его вычислить
#### Парсинг объектов
С помощью метода `eval()` можно парсть объекты, то есть преобразовывать их строки в реальные `Python` объекты.
```python
list_data = eval("['Python', 'C#', 'Java']")
tuple_data = eval('(1, 2, 3, 4, 5)')
dict_data = eval("{1: 'January', 2: 'February'}")

print(type(list_data), len(list_data))
print(type(tuple_data), max(tuple_data))
print(type(dict_data), dict_data[2])
```
```
<class 'list'> 3
<class 'tuple'> 5
<class 'dict'> February
```

Подробнее прочитать о функции `eval()` можно по [ссылке](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-eval/).

Использовать функции `eval()` и `exec()` нужно аккуратно, поскольку они являются небезопасными, так как выполняют произвольный код. Выполняемый код должен быть надежным и не содержать потенциальных угроз.