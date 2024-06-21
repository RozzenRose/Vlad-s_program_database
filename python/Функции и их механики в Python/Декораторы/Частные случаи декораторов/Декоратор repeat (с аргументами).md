#python #декоратор #repeat


Рассмотрим декоратор `repeat`, который вызывает декорируемую функцию переданное в качестве аргумента количество раз
```python
import functools

def repeater(repeat=1):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(1, repeat + 1):
                print(f'{i}-й запуск функции.')
                value = func(*args, **kwargs)
            return value
        return wrapper
    return decorator
```
Приведенный ниже код:
```python
@repeater(repeat=5)
def beegeek():
    print('beegeek')

beegeek()
```
выводит:
```
1-й запуск функции.
beegeek
2-й запуск функции.
beegeek
3-й запуск функции.
beegeek
4-й запуск функции.
beegeek
5-й запуск функции.
beegeek
```
Несмотря на то что декоратор `repeater` имеет значение по умолчанию для аргумента `repeat` применить его как стандартный декоратор, не указав скобки, мы не можем
```python
@repeater
def beegeek():
    print('beegeek')

beegeek()
```
приводит к возникновению ошибки.

Правильная версия кода имеет вид:
```python
@repeater()                       # используется значение по умолчанию repeat=1
def beegeek():
    print('beegeek')

beegeek()
```