#python #функция #_dict__


У объектов функций есть дополнительный атрибут `__dict__`, являющийся словарем и использующийся для динамического наделения функции дополнительным функционалом. Устанавливать и получать значения из данного атрибута можно, используя два синтаксиса.
- в стиле словаря: `func.__dict__['attr'] = value`
- через точечную нотацию: `func.attr = value`

Доступ к словарю атрибутов функции можно получить как из тела функции, так и извне
```python
def greet():
    greet.age = 17

print(greet.__dict__)

greet.value = 777
greet.numbers = [1, 2, 3]
greet.name = 'VlaDICK'

print(greet.__dict__)

greet()

print(greet.__dict__)
```
```
{}
{'value': 777, 'numbers': [1, 2, 3], 'name': 'VlaDICK'}
{'value': 777, 'numbers': [1, 2, 3], 'name': 'VlaDICK', 'age': 17}
```
Или вот еще пример:
```python
def countries(country, capital):
    countries.__dict__[country] = capital
    
countries.__dict__['Germany'] = 'Berlin'
countries.Norway = 'Oslo'
countries('Finland', 'Helsinki')

print(countries.__dict__)
```
```
{'Germany': 'Berlin', 'Norway': 'Oslo', 'Finland': 'Helsinki'}
```
Если никакие атрибуты функции никогда не устанавливались, то словарь атрибутов функции `__dict__` будет пустой

Словарь атрибутов может быть использован для кэширования уже вычисленных значений функции
```python
def fib(num):
    if num < 2:
        return num
    return fib(num - 1) + fib(num - 2)
```
многократно вычисляет числа Фибоначчи.

Используя атрибут `__dict__`, приведенный выше код можно оптимизировать:
```python
def fib(num):
    if num < 2:
        return num
    if num not in fib.__dict__:
        fib.__dict__[num] = fib(num - 1) + fib(num - 2)
    return fib.__dict__[num]
```