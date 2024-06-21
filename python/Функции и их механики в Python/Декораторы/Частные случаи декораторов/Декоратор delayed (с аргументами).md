#python #декоратор #delayed

Декоратор `delayed` создает требуемую задержку выполнения кода. Такое поведение иногда требуется для мониторинга доступности какого-нибудь ресурса
```python
import functools
import time

def delayed(delay=2):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print(f'Спим {delay} сек.')
            time.sleep(delay)
            value = func(*args, **kwargs)
            return value
        return wrapper
    return decorator
```
Задекорируем данным декоратором рекурсивную функцию обратного отсчета.
```python
@delayed(1)
def countdown(number):
    if number < 1:
        print('Конец!')
    else:
        print(number)
        countdown(number - 1)
        
countdown(5)
```
выводит (длительность работы программы примерно равна 6 секундам):
```
Спим 1 сек.
5
Спим 1 сек.
4
Спим 1 сек.
3
Спим 1 сек.
2
Спим 1 сек.
1
Спим 1 сек.
Конец!
```