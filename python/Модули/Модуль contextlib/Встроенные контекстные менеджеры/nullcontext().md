#модуль #python #контекстный_менеджер #contextlib #nullcontext


Для использования требует подключения модуля `contextlib`:
```python
from contextlib import nullcontext
```

Функция `nullcontext()` используется для построения пустых контекстных менеджеров, которые ничего не делают

Примерный код функции `nullcontext()`:
```python
from contextlib import contextmanager

@contextmanager
def nullcontext(enter_result=None):
    yield enter_result
```
Пример:
```python
from contextlib import nullcontext

with nullcontext(2077) as manager:
    print(manager)

with nullcontext('pygen') as manager:
    print(manager)
```
```
2077
pygen
```

Функцию `nullcontext()` удобно использовать для создания необязательного контекстного менеджера, например:
```python
from contextlib import suppress, nullcontext

def my_function(ignore_exceptions=False):
    if ignore_exceptions:
        manager = suppress(Exception)           # игнорируем исключения
    else:
        manager = nullcontext()                 # не игнорируем исключения
    
    with manager:
        # код
```