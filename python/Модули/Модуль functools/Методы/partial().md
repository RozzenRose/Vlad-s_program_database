#метод #functools #python #partial

Позволяет частично применять функции, как бы закидывать в них часть аргументов, сохранять их в таком виде, пока не понадобится закинуть в них оставшуюся часть переменных и выполнить
```python
from functools import partial
partial(func, *args, **kwargs)
```
Возвращает специальный `partial`-объект, который при вызове как функция `func`, в которую дополнительно передаются позиционные аргументы `args` и именованные аргументы `kwargs`.
```python
from functools import partial

def multiply(a, b):
    return a * b

double = partial(multiply, 2)
triple = partial(multiply, 3)
```
`partial` объекты `double` и `triple` ведут себя как функция `multiply()`, которой в качестве первого аргумента передали число 2 в первом случае и 3 во втором.  
Приведенный ниже код:
```python
print(double(5))        # 2 * 5
print(triple(10))       # 3 * 10
```
```
10
30
```
Через `partial()` был передан один аргумент, поэтому `partial` объекты ожидают только один аргумент. Если мы попытаемся передать более одного аргумента, то получим ошибку
```python
print(double(5, 6))
```
```
TypeError: multiply() takes 2 positional arguments but 3 were given
```
В то же время ничего не мешает передать через `partial()` сразу два аргумента
```python
​multiply_two_and_five = partial(multiply, 2, 5)
print(multiply_two_and_five())   # вызываем уже без аргументов
```
```
10
```
Тогда во время вызова нам уже не нужно передавать аргументы `partial` объекту
Это позволяет заменить следующий код:
```python
def multiply(a, b):
    return a * b

def double(num):
    return multiply(2, num)
 
def triple(num):
    return multiply(3, num)
```

При формировании новой функции с использованием `partial()` мы можем передавать не только позиционные аргументы, но и именованные. Вспомним, что встроенная функция `int()`, помимо конвертируемого значения, также принимает именованный аргумента `base`, имеющий по умолчанию значение 10. Данный аргумент отвечает за основание системы счисления конвертируемого значения
```python
print(int('123'))
print(int('123', base=5))
print(int('1001', base=2))
print(int('A12B', base=16))
```
```
123
38
9
41259
```
Зафиксировав значение именованного аргумента `base`, мы можем получить функцию `basetwo()`, которая переводит число из двоичной системы счисления в десятичную
```python
from functools import partial

basetwo = partial(int, base=2)

print(basetwo('101'))
print(basetwo('1000'))
print(basetwo('11111'))
```
```
5
8
31
```
Если другие аргументы передаются при вызове функции, то позиционные добавляются в конец, а именованные расширяются и перезаписываются.

#### Объекты, возвращаемые функцией partial()
Как уже было сказано выше, функция `partial()` возвращает специальные `partial` объекты, которые при вызове ведут себя как функции. Такие объекты содержат три полезных атрибута:
- `func` — исходная функция
- `args` — зафиксированные позиционные аргументы (тип `tuple`)
- `keywords` — зафиксированные именованные аргументы (тип `dict`)
При необходимости к ним можно получить доступ с помощью стандартной точечной нотации
```python
from functools import partial

def pretty_print(text, symbol, count):
    print(symbol * count)
    print(text)
    print(symbol * count)

star_pretty_print = partial(pretty_print, 'Hi!!!', symbol='*')

star_pretty_print(count=7)

print(star_pretty_print.args)
print(star_pretty_print.keywords)

star_pretty_print.func('Исходная функция', symbol='~', count=20)
```
```
*******
Hi!!!
*******
('Hi!!!',)
{'symbol': '*'}
~~~~~~~~~~~~~~~~~~~~
Исходная функция
~~~~~~~~~~~~~~~~~~~~
```
Как несложно заметить, частичное применение с помощью функции `partial()` работает подобно декоратору. При этом у `partial` объекта нет явных атрибутов `__name__` и `__doc__` от начальной функции. Доступ к этим атрибутам возможен только через исходную функцию с помощью атрибута `func`.
```python
from functools import partial

def multiply(a, b):
    '''Функция перемножает два числа и возвращает вычисленное значение.'''
    return a * b

double = partial(multiply, 2)

print(double.func.__name__)
print(double.func.__doc__)
```
```
multiply
Функция перемножает два числа и возвращает вычисленное значение.
```
Или можно применить метод [[update_wrapper()]]