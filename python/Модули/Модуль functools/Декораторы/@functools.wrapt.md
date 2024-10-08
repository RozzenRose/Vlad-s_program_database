#functools #python #wrapt #декоратор


Все функции содержат специальные атрибуты `__name__` и `__doc__`, которые содержат полезную информацию:
- `__name__` — имя функции
- `__doc__` — строка документации

Рассмотрим применение декоратора `bold` к функции `greet()`
```python
def bold(func):
    def wrapper(*args, **kwargs):
        return '<b>' + func(*args, **kwargs) + '</b>'
    return wrapper

@bold
def greet(name):
    '''Функция приветствия пользователя.'''
    return f'Hello {name}!'

print(greet.__name__)
print(greet.__doc__)
```
```
wrapper
None
```
После того как к функции `greet()` был применен декоратор, ее атрибуты `__name__` и `__doc__` изменились на имя и строку документации внутренней функции `wrapper()` декоратора `bold` Хотя чисто технически это верно, это не очень хорошо.

Для решения проблемы перетирания данных атрибутов на практике используют другой декоратор, который находится в модуле `functools` и называется `wraps`. Таким образом, чтобы предотвратить перетирание атрибутов `__name__` и `__doc__` декорируемой функции, декораторы должны использовать декоратор `functools.wraps`, который сохраняет информацию о первоначальной функции.

```python
import functools

def bold(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        return '<b>' + func(*args, **kwargs) + '</b>'
    return wrapper

@bold
def greet(name):
    '''Функция приветствие пользователя.'''
    return f'Hello {name}!'

print(greet.__name__)
print(greet.__doc__)
```
```no-highlight
greet
Функция приветствие пользователя.
```
Теперь у функции `greet()` атрибуты `__name__` и `__doc__` не перетираются после применения декоратора `bold`.