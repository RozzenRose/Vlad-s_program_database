#модуль #python #контекстный_менеджер #contextlib #ExitStack


Для использования требует подключения модуля `contextlib`:
```python
from contextlib import ExitStack
```

Контекстный менеджер `ExitStack` предназначен для объединения переменного числа контекстных менеджеров. Он удобен, когда заранее неизвестно, какое количество менеджеров контекста нужно обработать, например, в случае, когда одновременно открывается большое количество файлов из некоторого списка

Сам менеджер `ExitStack` представляет собой так называемую последовательность обратных вызовов. При входе в очередной контекстный менеджер его метод `__exit__()` добавляется в конец последовательности, но не вызывается. После завершения блока `with` менеджер `ExitStack` вызывает с конца все добавленные в последовательность методы `__exit__()`, то есть сперва вызывается последний добавленный метод, затем предпоследний, и так далее

#### Метод `enter_context()`
Метод `enter_context()` менеджера `ExitStack` принимает в качестве аргумента контекстный менеджер, входит в него и добавляет его метод `__exit__()` в конец последовательности обратных вызовов, но не вызывает
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


with ExitStack() as stack:
    stack.enter_context(Greeter('Гвидо'))
    stack.enter_context(Greeter('Трей'))
    print('Завершение блока with')
```
```
Привет, Гвидо
Привет, Трей
Завершение блока with
Пока, Трей
Пока, Гвидо
```
Возвращаемым значением метода `enter_context()` является возвращаемое значение метода `__enter__()` переданного контекстного менеджера
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


with ExitStack() as stack:
    name1 = stack.enter_context(Greeter('Гвидо'))
    name2 = stack.enter_context(Greeter('Трей'))
    print(name1, name2)
```
```
Привет, Гвидо
Привет, Трей
Гвидо Трей
Пока, Трей
Пока, Гвидо
```

#### push()
Метод `push()` менеджера `ExitStack` принимает в качестве аргумента контекстный менеджер и добавляет его метод `__exit__()` в конец последовательности обратных вызовов, но не вызывает
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


with ExitStack() as stack:
    stack.push(Greeter('Гвидо'))
    stack.push(Greeter('Трей'))
    print('Завершение блока with')
```
```
Завершение блока with
Пока, Трей
Пока, Гвидо
```
Обратите внимание, что метод `push()` не выполняет вход в переданный контекстный менеджер.

#### Метод `callback()`
В последовательность обратных вызовов можно добавлять не только методы `__exit__()` контекстных менеджеров, но и обыкновенные функции. Аналогично методам `__exit__()` эти функции будут вызваны после заверения блока `with`

Метод `callback()` менеджера `ExitStack` принимает в качестве аргумента функцию, любой набор из позиционных или именованных аргументов для этой функции, и добавляет эту функцию в конец последовательности обратных вызовов, но не вызывает
```python
from contextlib import ExitStack

def goodbye(name):
    print('Пока,', name)

with ExitStack() as stack:
    stack.callback(goodbye, 'Гвидо')
    stack.callback(goodbye, name='Трей')
    print('Завершение блока with')
```
```
Завершение блока with
Пока, Трей
Пока, Гвидо
```

#### Метод `close()`
Метод `close()` менеджера `ExitStack` немедленно опустошает последовательность обратных вызовов, вызывая с конца все имеющиеся в нем методы `__exit__()` и функции
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


def goodbye(name):
    print('Пока,', name)

with ExitStack() as stack:
    stack.enter_context(Greeter('Гвидо'))
    stack.push(Greeter('Трей'))
    stack.callback(goodbye, 'Алан')
    stack.close()
    print('Завершение блока with')
```
```
Привет, Гвидо
Пока, Алан
Пока, Трей
Пока, Гвидо
Завершение блока with
```

#### Метод `pop_all()`
Метод `pop_all()` переносит последовательность обратных вызовов в новый контекстный менеджер `ExitStack` и возвращает его
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


with ExitStack() as stack:
    stack.enter_context(Greeter('Гвидо'))
    stack.enter_context(Greeter('Трей'))
    stack.enter_context(Greeter('Алан'))
    new_stack = stack.pop_all()
    print('Завершение блока with')
```
```
Привет, Гвидо
Привет, Трей
Привет, Алан
Завершение блока with
```
Стоит обратить внимание, что исходная последовательность обратных вызовов становится пустой, поэтому после завершения блока `with` не выполняется ни одного вызова метода `__exit__()`. Теперь все они находятся в новом контекстном менеджере, и для их вызова необходимо воспользоваться именно им.
```python
from contextlib import ExitStack

class Greeter:
    def __init__(self, name):
        self.name = name
        
    def __enter__(self):
        print('Привет,', self.name)
        return self.name
    
    def __exit__(self, *args, **kwargs):
        print('Пока,', self.name)


with ExitStack() as stack:
    stack.enter_context(Greeter('Гвидо'))
    stack.enter_context(Greeter('Трей'))
    stack.enter_context(Greeter('Алан'))
    new_stack = stack.pop_all()
    print('Завершение блока with')
    
new_stack.close()
```
```
Привет, Гвидо
Привет, Трей
Привет, Алан
Завершение блока with
Пока, Алан
Пока, Трей
Пока, Гвидо
```