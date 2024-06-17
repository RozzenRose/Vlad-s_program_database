#встроенная_функция #python #метод #exec

Метод `exec()` принимает блок кода и выполняет его, возвращая значение `None`. Похож на `eval()`, но прикольнее, имеет всего один аргумент `code`
```python
exec(code)
```

Пример:
```python
code = '''a = 10
b = 20
print(a + b)'''

exec(code)
```
```
30
```
Важно помнить, что функция `exec()` именно выполняет переданный блок кода и всегда возвращает значение `None`
```python
code = '100 + 10*7 - 14'

result = exec(code)

print(result)
```
```
None
```
Блок кода, передаваемый в качестве аргумента функции `exec()`, имеет доступ ко всем встроенным функциям Python
```python
code1 = 'print(sorted([3, 5, 4, 1, 2]))'
code2 = 'print(sum([3, 5, 4, 1, 2]))'
code3 = 'print(len([3, 5, 4, 1, 2]))'

exec(code1)
exec(code2)
exec(code3)
```
```
[1, 2, 3, 4, 5]
15
5
```
Блок кода, передаваемый в качестве аргумента функции `exec()`, имеет доступ ко всем локальным и глобальным переменным.
```python
numbers = [1, 2, 3, 4, 5]
info = {'name': 'Timur', 'surname': 'Guev'}

code1 = '''total = 0
for i in numbers:
    total += i
print(total)'''
code2 = 'print(info["name"], info["surname"])'

exec(code1)
exec(code2)
```
```
15
Timur Guev
```
 Подробнее прочитать о функции `exec()` можно по [ссылке](https://docs-python.ru/tutorial/vstroennye-funktsii-interpretatora-python/funktsija-exec/).
 
Использовать функции `eval()` и `exec()` нужно аккуратно, поскольку они являются небезопасными, так как выполняют произвольный код. Выполняемый код должен быть надежным и не содержать потенциальных угроз.