#collections #Python #OrderedDict #упорядоченные_словари


Тип `OrderedDict` является подтипом типа `dict`, сохраняющий порядок, в котором пары "ключ-значение" вставляются в словарь. Когда мы перебираем объекты типа `OrderDict`, его элементы перебираются в исходном порядке. Если мы обновим значение существующего ключа, то порядок останется неизменимым. Если мы удалим элемент и вставим его снова, то этот  элемент будет добавлен в конце словаря.

Тип `OrderedDict` будучи подтипом `dict` наследует все методы, предоставляемые обычным словарем. При этом в `OrderedDict` также есть дополнительные методы.

В отличии от `Dict` тип `OrderedDict` не является встроенным типом и для использования его необходимо импортировать из модуля `collections`.
```python
from collections import OrderedDict

numbers = OrderedDict()

numbers['one'] = 1
numbers['two'] = 2
numbers['three'] = 3

print(numbers)
```
```
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
```
Как и `defaultdict`, эти словари можно создавать любым из доступных способов, как и обычные словари:
```python
from collections import OrderedDict

numbers1 = OrderedDict({'one': 1, 'two': 2, 'three': 3})
numbers2 = OrderedDict([('one', 1), ('two', 2), ('three', 3)])
numbers3 = OrderedDict(one=1, two=2, three=3)
```

### Изменение `OrderedDict` словаря
Тип `OrderedDict` является изменяемым. Мы можем вставлять новые элементы, обновлять и удалять существующие элементы. Если мы вставим новый элемент в существующий `OrderedDict` словарь, то этот элемент добавится в конец словаря.
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

numbers['four'] = 4
print(numbers)
```
```
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('one', 1), ('two', 2), ('three', 3), ('four', 4)])
```

Если мы удалим элемент из существующего `OrderedDict` словаря и снова вставим его, то он будет помещен в конце словаря.
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

del numbers['one']

print(numbers)
numbers['one'] = 1
print(numbers)
```
```
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3), ('one', 1)])
```

Если мы обновляем значение по существующему ключу, то ключ сохраняет свою позицию.
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

numbers['one'] = 1.0
print(numbers)

numbers.update(two=2.0)
print(numbers)
```
```
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('one', 1.0), ('two', 2), ('three', 3)])
OrderedDict([('one', 1.0), ('two', 2.0), ('three', 3)])
```
Обновить значение по нужному ключу можно либо с помощью квадратных скобок, либо с помощью словарного метода `update()`


### Итерирование по `OrderedDict` словарю
Доступ к элементам и итерирование по `OrderedDict` словарям работает так же, как и у обычных словарей. Мы можем перебирать ключи напрямую или можем использовать словарные методы `items()`, `keys()` и `value()`
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

# обращение по ключу
print(numbers['one'])
print(numbers['three'])
print()

# перебор ключей напрямую
for key in numbers:
    print(key, '->', numbers[key])
print()

# перебор пар (ключ, значение) через метод
for key, value in numbers.items():
    print(key, '->', value)
print()

# перебор ключей через метод
for key in numbers.keys():
    print(key, '->', numbers[key])
print()

# перебор значений через метод
for value in numbers.values():
    print(value)
```
```
1
3

one -> 1
two -> 2
three -> 3

one -> 1
two -> 2
three -> 3

one -> 1
two -> 2
three -> 3

1
2
3
```

При итерировании по `OrderedDict` словарям мы можем использовать встроенную функцию `reversed()`
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

# перебор ключей напрямую
for key in reversed(numbers):
    print(key, '->', numbers[key])
print()

# перебор пар (ключ, значение) через метод
for key, value in reversed(numbers.items()):
    print(key, '->', value)
print()

# перебор ключей через метод
for key in reversed(numbers.keys()):
    print(key, '->', numbers[key])
print()

# перебор значений через метод
for value in reversed(numbers.values()):
    print(value)
```
```
three -> 3
two -> 2
one -> 1

three -> 3
two -> 2
one -> 1

three -> 3
two -> 2
one -> 1

3
2
1
```


### Методы `popitem()` и `move_to_end()`
`OrderedDict` словари имеют два полезных метода:
- метод `move_to_end()` позволяет переместить существующий элемент либо в конец, либо в начало словаря
- метод `popitem()` позволяет удалить и вернуть элемент либо из конца, либо из начала словаря

#### Метод `move_to_end()`
Методу `move_to_end()` можно передать два аргумента:
- `key` (обязательный аргумент) – ключ, который идентифицирует перемещаемый элемент
- `last` (необязательный аргумент) – логическое значение (тип `bool`), которое определяет, в какой конец словаря мы перемещаем элемент, значение `True` (по умолчанию) перемещает элемент в конец, значение `False` – в начало

Если при вызове метода `move_to_end()` переданный ключ отсутствует в словаре, то возникнет ошибка `KeyError`