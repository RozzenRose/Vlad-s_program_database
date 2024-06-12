#python #встроенная_функция #help #метод

Метод `help()` используется для получения документации по указанному модулю, функции или другому объекту. Она принимает в качестве аргумента либо сам объект, либо строку с именем объекта. Вызов без аргументов запускает интерактивную справочную систему в консоли интерпретатора (для выхода используй команду `quit`)
```python
help(print)
help('sorted')
```
```
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.

Help on built-in function sorted in module builtins:

sorted(iterable, /, *, key=None, reverse=False)
    Return a new list containing all items from the iterable in ascending order.
    
    A custom key function can be supplied to customize the sort order, and the
    reverse flag can be set to request the result in descending order.
```