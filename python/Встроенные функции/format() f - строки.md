#встроенная_функция #python

Позволяет вставлять в значения типа str значения внешних переменных.
```python
name = 'Vald'
text = 'Gratest programmer of the world it is {}'.format(name)
```
Можно вкраплять в ряд
```python
name = 'Vlad'
spec = 'programmer'
language = 'Python'
text = 'Gratest {} on {} of the world it is {}'.format(spec, language, name)
```
Можно еще нумеровать вкрапления
```python
name = 'Vlad'
spec = 'programmer'
language = 'Arch'
text = "Gratest {2}'s {0} of the world it is {3}".format(spec, language, name)
```
f-строки делают примерно тоже самое, вот синтаксис:
```python
name = 'Vlad'
spec = 'programmer'
language = 'Python'
text = f'Gratest {spec} on {language} of the world it is {name}'
```

Интересно, что этот функционал на место {} возвращает значение типа `str`. И вообще то это позволяет сокращать числа с плавающей точкой типов float или decimal.
```python
number = 3.456
round_number = '{:2f}'.format(number) #оругление до 2 цифр после запятой
#OR
round_number = f'{number:.2f}' #тоже самое
```