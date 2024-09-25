#модуль #python #контекстный_менеджер #contextlib #contextmanager 


Требует подключения модуля `contextlib`:
```python
from contextlib import contextmanager
```
В `Python` создавать контекстные менеджеры можно при помощи написания кастомных классов, но намного проще с помощью декоратора `@contextmanager`. Декоратор `@contextmanager` позволяет создавать контекстный менеджер на основе функции, автоматически предоставляя оба требуемых метода `__enter__()` и `__exit__()`

Контекстный менеджер `CustomContextManager`, который просто выводит текст в методах `__enter__()` и `__exit__()`:
```python
class CustomContextManager:
    def __enter__(self):
        print('Вход в контекстный менеджер...')
        return 'Python generation!'

    def __exit__(self, exc_type, exc_value, traceback):
        print('Выход из контекстного менеджера...')
```
с использованием декоратора `@contextmanager` можно переписать в виде:
```python
from contextlib import contextmanager

@contextmanager
def custom_context_manager():
    print('Вход в контекстный менеджер...')
    yield 'Python generation!'
    print('Выход из контекстного менеджера...')
```
Приведенный ниже код:
```python
with custom_context_manager() as manager:
    print(manager)
```
```
Вход в контекстный менеджер...
Python generation!
Выход из контекстного менеджера...
```
Как мы видим, код до оператора `yield` выполняется, когда поток выполнения входит в контекст и соответствует методу `__enter__()`. Значение, которое возвращает оператор `yield` является возвращаемым значением метода `__enter__()`. Наконец, код после оператора `yield` выполняется, когда поток выполнения выходит из контекста, что соответствует методу `__exit__()`.

Декоратор `@contextmanager` – элегантный и практически полезный инструмент, который сводит воедино три совершенно разных механизма Python: декоратор функции, генератор и оператор `with`.

Обычно с помощью декоратора `@contextmanager` создают **относительно несложные** контекстные менеджеры.

Функция, к которой применяется декоратор `@contextmanager`, обязательно должна иметь инструкцию `yield`, в противном случае при попытке воспользоваться реализованной функцией как контекстным менеджером будет возбуждено исключение.

Контекстные менеджеры созданные с помощью декоратора `@contextmanager` являются **одноразовыми**, их нужно создавать заново каждый раз, когда мы хотим их использовать. Попытка повторного использования такого контекстного менеджера приводит к возбуждению исключения `AttributeError`.

Процесс создания контекстных менеджеров на основе функций похож на процесс создания итераторов с помощью генераторных функций. Подробнее о генераторных функциях можно почитать по [ссылке](https://stepik.org/lesson/640048/step/1?unit=636568).

 При использовании контекстных менеджеров на основе классов мы могли достаточно легко передавать значения из метода `__enter__()` в тело блока `with`. Для этого нужно было использовать атрибуты объекта, возвращаемого методом `__enter__()`. При использовании контекстных менеджеров на основе функций мы можем добиться аналогичного поведения, если будем возвращать лямбда-функцию

### Примеры:
#### Пример 1
Контекстный менеджер `Trace` выводит информацию перед входом в блок `with` и после выхода из блока `with`, включая информацию об исключении, если оно бывало возбуждено во время выполнения блоки `with`
```python
class Trace:
    def __enter__(self):
        print('Начало выполнения блока with')

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_value:
            print(f'Во время выполнения блока with возникло исключение {exc_value}')
        print('Конец выполнения блока with')
        return True                           # обрабатываем все типы исключений
```
С помощью декоратора `@contextmanager` контекстный менеджер `Trace` выглядит так:
```python
from contextlib import contextmanager

@contextmanager
def trace():
    print('Начало выполнения блока with')
    try:
        yield
    except Exception as error:
        print(f'Во время выполнения блока with возникло исключение {error}')
    finally:
        print('Конец выполнения блока with')
```
Используем контекстные менеджеры:
```python
with trace():
    print('Python generation!')

print()

with trace():
    print('Python generation!')
    print(1 / 0)
```
```
Начало выполнения блока with
Python generation!
Конец выполнения блока with

Начало выполнения блока with
Python generation!
Во время выполнения блока with возникло исключение division by zero
Конец выполнения блока with
```
#### Пример 2
Контекстный менеджер `WritableTextFile` позволяет работать с открытыми для записи текстовыми файлами в кодировке `UTF-8`:
```python
class WritableTextFile:
    def __init__(self, path):
        self.path = path

    def __enter__(self):
        self.file = open(self.path, mode='w', encoding='utf-8')
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        self.file.close()
```
С помощью декоратора `@contextmanager` контекстный менеджер `WritableTextFile` выглядит так:
```python
from contextlib import contextmanager

@contextmanager
def writable_text_file(path):
    file = open(path, mode='w', encoding='utf-8')
    yield file
    file.close()
```
Приведенный ниже код:
```python
with writable_text_file('output.txt') as file:
    file.write('Python generation!')
```
создает текстовый файл `output.txt` в кодировке `UTF-8` с содержимым `Python generation!`.

#### Пример 3
Контекстный менеджер `RedirectedStdiut` __временно__ перенаправляет стандартный вывод `sys.stdout` на некоторый файл на диске:
```python
import sys

class RedirectedStdout:
    def __init__(self, new_output):
        self.new_output = new_output

    def __enter__(self):
        self.standart_output = sys.stdout
        sys.stdout = self.new_output

    def __exit__(self, exc_type, exc_value, traceback):
        sys.stdout = self.standart_output 
```
С помощью декоратора `@contextmanager` контекстный менеджер `RedirectedStdout` выглядит так:
```python
import sys
from contextlib import contextmanager

@contextmanager
def redirected_stdout(new_output):
    standart_output = sys.stdout
    sys.stdout = new_output
    yield
    sys.stdout = standart_output
```
Приведенный ниже код:
```python
with open('output.txt', mode='w', encoding='utf-8') as file:
    with redirected_stdout(file):
        print('Python generation!')
    print('Возврат к стандартному потоку вывода')
```
создает текстовый файл `output.txt` в кодировке `UTF-8` с содержимым `Python generation!`, а также выводит текст `Возврат к стандартному потоку вывода`.
#### Пример 4
 Контекстный менеджер `Timer` позволяет измерять время выполнения блока кода:
```python
from time import perf_counter

class Timer:
    def __enter__(self):
        self.start = perf_counter()
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        self.elapsed = perf_counter() - self.start
```
С помощью декоратора `@contextmanager` контекстный менеджер `Timer` выглядит так:
```python
from time import perf_counter
from contextlib import contextmanager

@contextmanager
def timer():
    start = perf_counter()
    yield
    end = perf_counter()
    elapsed = end - start
    print('Затраченное время:', elapsed)
```
Приведенный ниже код:
```python
from time import sleep

with timer():
    sleep(0.7)
    sleep(1.5)
```
выводит (время может отличаться):
```no-highlight
Затраченное время: 2.208050799788907
```