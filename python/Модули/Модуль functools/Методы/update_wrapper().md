#метод #functools #python #partial 

Позволяет добавить атрибуты `__name__` и `__doc__` из исходной функции в [[partial()]] объект:
```python
from functools import partial, update_wrapper

def multiply(a, b):
    '''Функция перемножает два числа и возвращает вычисленное значение.'''
    return a * b

double = partial(multiply, 2)
update_wrapper(double, multiply)   # копируем информацию из функции multiply в partial объект double

print(double.__name__)
print(double.__doc__)
```
```
multiply
Функция перемножает два числа и возвращает вычисленное значение.
```

Прочитать подробнее о функции `update_wrapper()` можно по [ссылке](https://docs-python.ru/standart-library/modul-functools-python/dekorator-update-wrapper-modulja-functools/). 