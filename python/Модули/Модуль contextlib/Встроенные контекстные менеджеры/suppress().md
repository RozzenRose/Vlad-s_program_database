#модуль #python #контекстный_менеджер #contextlib #suppress


Для использования требует подключения модуля `contextlib`:
```python
from contextlib import suppress
```

Функция `suppress()` используется для построения контекстных менеджеров, которые игнорируют заданные типы исключений

Примерный код функции `suppress()`
```python
from contextlib import contextmanager

@contextmanager
def suppress(*exceptions):
    try:
        yield
    except Exception as error:
        if type(error) not in exceptions:
            raise
```
Пример:
```python
from contextlib import suppress

with suppress(ValueError):
    num = int('python')

print('beegeek')

with suppress(TypeError, ZeroDivisionError):
    num = 1 / 0

print('pygen')
```
```
beegeek
pygen
```
Как и с любым другим механизмом, который полностью подавляет исключения, этот контекстный менеджер следует использовать только для покрытия специальных ошибок, когда точно известно, что продолжение работы программы без вывода сообщений будет корректным

Контекстный менеджер, возвращаемый функцией `suppress()`, является реентерабельным