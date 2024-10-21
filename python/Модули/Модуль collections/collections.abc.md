#collections #Python #abc #объект


Модуль `collections.abc` предоставляет абстрактные базовые классы, которые можно использовать для проверки того, реализует объект тот или иной интерфейс или нет

| Класс (Интерфейс) | Наследуется от               | Абстрактные методы                                                   | Mixin методы                                                                                                          |
| ----------------- | ---------------------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `Container`       |                              | `__contains__()`                                                     |                                                                                                                       |
| `Hashable`        |                              | `__hash__()`                                                         |                                                                                                                       |
| `Iterable`        |                              | `__iter__()`                                                         |                                                                                                                       |
| `Iterator`        | `Iterable`                   | `__next__()`                                                         | `__iter__()`                                                                                                          |
| `Reversible`      | `Iterable`                   | `__reversed__()`                                                     |                                                                                                                       |
| `Generator`       | `Iterator`                   | `send(), throw()`                                                    | `close(), __iter__(), __next__()`                                                                                     |
| `Sized`           |                              | `__len__()`                                                          |                                                                                                                       |
| `Callable`        |                              | `__call__()`                                                         |                                                                                                                       |
| `Collection`      | `Sized, Iterable, Container` | `__contains__(), __iter__(), __len__()`                              |                                                                                                                       |
| `Sequence`        | `Reversible, Collection`     | `__getitem__(), __len__()`                                           | `__contains__(), __iter__(), __reversed__(), index(), count()`                                                        |
| `MutableSequence` | `Sequence`                   | `__getitem__(), __setitem__(), __delitem__(), __len__(), insert()`   | все методы `Sequence, append(), reverse(), extend(), pop(), remove(), __iadd__()`                                     |
| `Set`             | `Collection`                 | `__contains__(), __iter__(), __len__()`                              | `__le__(), __lt__(), __eq__(), __ne__(), __gt__(), __ge__(), __and__(), __or__(), __sub__(), __xor__(), isdisjoint()` |
| `MutableSet`      | `Set`                        | `__contains__(), __iter__(), __len__(), add(), discard()`            | все методы `Set, clear(), pop(), remove(), __ior__(), __iand__(), __ixor__(), __isub__()`                             |
| `Mapping`         | `Collection`                 | `__getitem__(), __iter__(), __len__()`                               | `__contains__(), keys(), items(), values(), get(), __eq__(), __ne__()`                                                |
| `MutableMapping`  | `Mapping`                    | `__getitem__(), __setitem__(), __delitem__(), __iter__(), __len__()` | все методы `Mapping, pop(), popitem(), clear(), update(), setdefault()`                                               |
`Mixin` методы представляют собой методы, которые автоматически реализуются на основе абстрактных методов

Таким образом, некоторый объект реализует интерфейс:
- `Iterable`, если он содержит метод `__iter__()`
- `Iterator`, если он содержит методы `__next__()` и `__iter__()`
- `Collection`, если он содержит методы `__contains__(), __iter__()` и `__len__()`
- `Sequence`, если он содержит методы `__getitem__(), __len__(), __contains__(), __iter__(), __reversed__(), index()` и `count()`
- `Mapping`, если он содержит методы `__getitem__(), __iter__(), __len__(), __contains__(), keys(), items(), values(), get(), __eq__()` и `__ne__()`
- и т.д.

С помощью типов из данного модуля и функций `isinstance()` и `issubclass()` можно проверить, например, является ли объект итерируемым, или, например, реализует ли класс протокол последовательности
```python
from collections.abc import Sequence, MutableSequence

print(isinstance([1, 2, 3], Sequence))
print(isinstance(['one', 'two'], MutableSequence))

print(issubclass(list, Sequence))
print(issubclass(list, MutableSequence))
```
```
True
True
True
True
```

```python
from collections.abc import Sequence, MutableSequence

print(isinstance((1, 2, 3), Sequence))
print(isinstance(('one', 'two'), MutableSequence))

print(issubclass(tuple, Sequence))
print(issubclass(tuple, MutableSequence))
```
```
True
False
True
False
```

```python
from collections.abc import Sequence, MutableSequence

print(isinstance('beegeek', Sequence))
print(isinstance('beegeek', MutableSequence))

print(issubclass(str, Sequence))
print(issubclass(str, MutableSequence))
```
```
True
False
True
False
```

```python
from collections.abc import Mapping, MutableMapping, MutableSequence

print(isinstance({'one': 1, 'two': 2}, MutableSequence))
print(isinstance({'one': 1, 'two': 2}, Mapping))
print(isinstance({'one': 1, 'two': 2}, MutableMapping))

print(issubclass(dict, MutableSequence))
print(issubclass(dict, Mapping))
print(issubclass(dict, MutableMapping))
```
```
False
True
True
False
True
True
```

### Использование `collections.abc`
Абстрактные базовые классы модуля `collections.abc` полезны при создании пользовательских классов, которые работают как последовательности (`tuple`, `str`, `list`), множества (`set`), отображая (`dict`) и т.д. иногда вместо наследования от встроенных типов `tuple` `str` `set` `list` `dict` куда проще реализовать соответствующий интерфейс

Создадим класс `ListBasedSet`, который представляет собой неизменяемое множество, основанное на списке. Такая реализация будет медленнее обычного множества, но при этом будет потреблять меньше памяти:
```python
from collections.abc import Set

class ListBasedSet(Set):
    def __init__(self, iterable):
        lst = []
        for value in iterable:
            if value not in lst:
                lst.append(value)
        self.elements = lst

    def __iter__(self):
        return iter(self.elements)

    def __contains__(self, value):
        return value in self.elements

    def __len__(self):
        return len(self.elements)
    
    def __repr__(self):
        return f'{type(self).__name__}({self.elements})'
```
```python
s1 = ListBasedSet('abcdef')
s2 = ListBasedSet('defghi')

print(s1 | s2)
print(s1 & s2)
print(s1 - s2)
```
```
ListBasedSet(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i'])
ListBasedSet(['d', 'e', 'f'])
ListBasedSet(['a', 'b', 'c'])
```
Стоит обратить внимание, что нам достаточно реализовать лишь методы `__iter__()` `__contains__()` и `__len__()`. При этом методы `__le__()` `__lt__()` `__eq__()` `__ne__()` `__gt__()` `__and__()` `__or__()` `__sub__()` `__xor__()` `isdisjoint()`, а вместе с ними и соответствующие операторы будут реализованы автоматически

Помимо абстрактных методов, наследники классов из `collections.abc` могут содержать любой дополнительный набор полезных для них методов



**Примечание 1.** Документация по модулю `collections.abc` доступна по [ссылке](https://docs.python.org/3/library/collections.abc.html).

**Примечание 2.** Статья, описывающая основные возможности модуля `collections.abc` доступна по [ссылке](https://peps.python.org/pep-3119/).

**Примечание 3.** Абстрактные базовые классы позволяют нам проверять наличие определенного функционала у объектов. Например, если мы хотим проверить наличие метода `__len__()`, то вместо кода:
```python
size = None

if hasattr(size, '__len__'):
    size = len(size)
```
можем написать более типизированный код:
```python
from collections.abc import Sized

size = None

if isinstance(size, Sized):
    size = len(size)
```

**Примечание 4.** Если при создании пользовательских классов, которые работают подобно последовательностям (`tuple, str, list`), множествам (`set`), отображениям (`dict`) нужно менять их основные функции (нестандартный функционал), унаследованые от базовых классов, то наследование может оказаться болезненным, поскольку придется переопределять много методов. В этих ситуациях проще создать класс, унаследованный от одного из абстрактных классов модуля `collections.abc`. Если же мы создаем пользовательский класс, который совсем немного отличается от своего базового, то можно рассмотреть наследование от типов `list, str, dict, UserList, UserString, UserDict`.

**Примечание 5.** В Python используется утиная типизация. Таким образом, для того, чтобы объекты пользовательских классов поддерживали определенный функционал, им достаточно содержать лишь необходимые методы. Скажем, для того чтобы иметь возможность обращаться к объекту через квадратные скобки `[]`, достаточно иметь метод `__getitem__()`. При этом Python не требует, чтобы соответствующие классы были унаследованы от `Sequence` или `Mapping`. Однако нужно понимать, что использование модуля `collections.abc` все-таки дает определенные преимущества. Во-первых, мы получаем возможность разделять разные типы данных по их возможностям, а во-вторых мы получаем уже реализованные методы на основе абстрактных методов. Например, унаследовавшись от `Sequence`, нам достаточно реализовать методы `__getitem__()` и `__len__()`. При этом методы  `__contains__(), __iter__(), __reversed__(), index()`  и `count()` будут реализованы автоматически.

**Примечание 6.** Не забывайте, что наследник абстрактного класса должен переопределить **все** абстрактные методы, иначе экземпляр такого класса будет невозможно создать.