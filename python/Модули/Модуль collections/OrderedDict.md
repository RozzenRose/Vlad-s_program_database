#collections #Python #OrderedDict #упорядоченные_словари #popitem #move_to_end


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

При сравнении двух объектов `dict` порядок не имеет значения. При сравнении двух объектов `OrderedDict` порядок имеет значение. При сравнении `dict` и `OrderedDict` порядок не имеет значения.

Тип данных `OrderedDict` проигрывает типу `dict` по производительности:
- примерно на 40%40% медленнее
- примерно на 50%50% занимает больше памяти

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
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)
print(numbers)

numbers.move_to_end('one')       # last=True
print(numbers)

numbers.move_to_end('three', last=False)       # last=False
print(numbers)
```
```
OrderedDict([('one', 1), ('two', 2), ('three', 3)])
OrderedDict([('two', 2), ('three', 3), ('one', 1)])
OrderedDict([('three', 3), ('two', 2), ('one', 1)])
```
С помощью метода `move_to_end()` мы можем сортировать `OrdfderedDict` словарь по ключам.
```python
from collections import OrderedDict

letters = OrderedDict(b=2, d=4, a=1, c=3)

for key in sorted(letters):
    letters.move_to_end(key)

print(letters)
```
```
OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```

#### Метод popitem()
Метод `popitem()` по умолчанию удаляет и возвращает элемент с конца словаря.
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

print(numbers.popitem())
print(numbers)

print(numbers.popitem())
print(numbers)
```
```
('three', 3)
OrderedDict([('one', 1), ('two', 2)])
('two', 2)
OrderedDict([('one', 1)])
```
Если этому методу передать необязательный аргумент `last=False`, то он начнет удалять и возвращать элементы в порядке `FIFO`(First-In/First-Out, первый пришел/первый ушел) 
```python
from collections import OrderedDict

numbers = OrderedDict(one=1, two=2, three=3)

print(numbers.popitem(last=False))
print(numbers)

print(numbers.popitem(last=False))
print(numbers)
```
```
('one', 1)
OrderedDict([('two', 2), ('three', 3)])
('two', 2)
OrderedDict([('three', 3)])
```
### Различия в `dict()` методах


|Функционал|Тип OrderedDict|Тип dict|
|---|---|---|
|сохранность порядка вставки ключей|да (начиная с Python 3.1)|да (начиная с Python 3.6)|
|удобочитаемость и сигнализация о намерениях|высокая|низкая|
|возможность менять порядок элементов|да (методы `move_to_end()`, `popitem()`)|нет|
|производительность операций|низкая|высокая|
|потребление памяти|высокое|низкое|
|учет порядка элементов при сравнении на равенство|да|нет|
|перебор ключей в обратном порядке|да (начиная с Python 3.5)|да (начиная с Python 3.8)|
|возможность добавления пользовательских атрибутов|да (атрибут `.__dict__`)|нет|
|возможность использовать операторы `\|` и `\|=`|да (начиная с Python 3.9)|да (начиная с Python 3.9)|
