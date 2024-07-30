#python #re #fullmatch #метод


Функция `fullmatch()` возвращает специальный объект соответствия (тип `Match`), если **вся строка** соответствует регулярному выражению, или значение `None` в противном случае.

Аргументы функции:
- `pattern` — шаблон регулярного выражения
- `string` — строка для поиска
- `flags=0` — один или несколько флагов (необязательный аргумент)

```python
from re import fullmatch

match1 = fullmatch('\d+', '123foo')
match2 = fullmatch('\d+', 'foo123')
match3 = fullmatch('\d+', 'foo123bar')
match4 = fullmatch('\d+', '123')

print(match1)
print(match2)
print(match3)
print(match4)
```
```
None
None
None
<re.Match object; span=(0, 3), match='123'>
```
Регулярному выражению `\d+` соответствует последовательность цифр. Из всех четырех строк в приведенном выше примере только строка `123` полностью состроит их цифр.

Обратите внимание на то, что мы можем эмулировать работу функции `fullmatch()` с помощью `search()`, добавив в шаблон регулярного выражения метасимволы начала (символ `^`) и конца строки (символ `$`)

```python
from re import search

match = search('^\d+$', '123')

print(match)
```
```
<re.Match object; span=(0, 3), match='123'>
```

Функцию `fullmatch()` удобно использовать для валидации правильности данных
```python
from re import fullmatch

phone_pattern = '\d{3}-\d{3}-\d{4}'

match1 = fullmatch(phone_pattern , '777-888-1234')
match2 = fullmatch(phone_pattern , '77-89-56')
match3 = fullmatch(phone_pattern , '5555-99-1234')
match4 = fullmatch(phone_pattern , '123-000-3333ab')

print(match1)
print(match2)
print(match3)
print(match4)
```
проверяет соответствие строк регулярному выражению `ddd-ddd-dddd` (три цифры дефис три цифры дефис четыре цифры) и выводит:
```
<re.Match object; span=(0, 12), match='777-888-1234'>
None
None
None
```