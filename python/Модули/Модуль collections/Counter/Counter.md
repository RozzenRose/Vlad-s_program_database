#collections #Python #Counter #объект


Это специальный тип данных, который создан для того, что бы считать объекты

Тип `Counter` является подтипом типа `dict`, специально разработанный для подсчета хешируемых объектов в Python. `Counter` хранит объекты в качестве ключей, а их количество - в качестве значений.

Использует высокооптимизированную функцию, написанную на языке C. Поэтому беспокоиться об эффективности использования данного типа не стоит.
### [[Методы Counter]]
Помимо доступных для всех словарей методов, словарь `Counter` поддерживает еще четыре дополнительных:
- `most_common()` - самые часто встречающиеся значения
- `elements()` - получение ключей
- `total()` (начиная с Python 3.10) - сумма всех значений типа `Counter`
- `subtract()` - 
### Создание `Counter`
Существует несколько способов создания такого объекта. Самый простой способ основан на передаче коллекции с данными в конструктор типа.
```python
from collections import Counter

counter = Counter('mississippi')     # создаем счетчик на основе строки
print(counter)
```
```
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```
При этом `Counter` можно преобразовать в обычный словарь при помощи функции `dict()`.

Мы можем создавать объекты типа `Counter`, явно указывая начальные значения количества объектов.
```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(i=4, s=4, p=2, m=1)

print(counter1)
print(counter2)
```
```
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```
Тип `Counter`, будучи подтипом типа `dict`, наследует все методы, предоставляемые обычным словарем. При этом вызов метода  `fromkeys()` всегда будет приводить к возникновению ошибки:
```
NotImplementedError: Counter.fromkeys() is undefined.  Use Counter(iterable) instead.
```
Такое поведение неслучайно , оно позволяет избежать ошибок неоднозначности при создании объектов типа `Counter`.

На ключи накладываются такие же ограничения, как и на ключи для обычных словарей: ключи должны быть хешируемыми.  При этом нет никаких ограничений на объекты, которые мы будем хранить в качестве значений.
```python
from collections import Counter

inventory = Counter(apple=5, orange=7, banana=0, tomato=-2, watermelon='N/A')

print(inventory)
```
```
Counter({'apple': 5, 'orange': 7, 'banana': 0, 'tomato': -2, 'watermelon': 'N/A'})
```
Тип `Counter` позволяет нам писать такой код, однако нужно понимать, что для правильной работы в качестве подсчета объектов значения должны быть целыми неотрицательными числами, представляющими количество.


### Обновление данных
Для изменения объектов типа `Counter` мы можем использовать метод `update()`. Реализация данного метода не заменяет значения как у обычных словарей, а суммирует существующий значения. При этом для новых объектов метод `update()` создает новые пары `ключ: количество
```python
from collections import Counter

letters = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
print(letters)

letters.update('missouri')
print(letters)
```
```
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
Counter({'i': 6, 's': 6, 'p': 2, 'm': 2, 'o': 1, 'u': 1, 'r': 1})
````
Методу `update()` можно передать другой объект `Counter`.
```python
from collections import Counter

sales = Counter(apple=20, orange=5, banana=10)
monday_sales = Counter(apple=3, orange=12, banana=7)
tuesday_sales = {'apple': 4, 'orange': 5, 'tomato': 6}

print(sales)

sales.update(monday_sales)
print(sales)

sales.update(tuesday_sales)
print(sales)
```
```
Counter({'apple': 20, 'banana': 10, 'orange': 5})
Counter({'apple': 23, 'orange': 17, 'banana': 17})
Counter({'apple': 27, 'orange': 22, 'banana': 17, 'tomato': 6})
```
Мы так же можем использовать метод `update()` с именованными аргументами. К примеру, вызов метода `sales.update(apple=3, orange=12, banana=7)` равнозначен вызову метода `sales.update(moneday_sales)`.

Методом `clear()` можно удалить все элементы из списка.
```python
from collections import Counter

counter = Counter(i=4, s=40, a=1, p=20, b=98, z=69)

counter.clear()

print(counter)
``` 
```
Counter()
```

### Доступ к элементам и итерирование
Доступ к элементам и итерирование по `Counter` словарям работает так же, как и у обычных словарей. Мы можем перебирать ключи напрямую или можем использовать словарные методы `items()`, `keys()`, `values()`.
```python
from collections import Counter

letters = Counter('mississippi')

# обращение по ключу
print(letters['p'])
print(letters['i'])

print()

# перебор ключей напрямую
for letter in letters:
    print(letter, '->', letters[letter])

print()

# перебор ключей через метод
for letter in letters.keys():
    print(letter, '->', letters[letter])

print()

# перебор значений через метод
for count in letters.values():
    print(count)

print()

# перебор пар (ключ, значение) через метод
for letter, count in letters.items():
    print(letter, '->', count)
```
```
2
4

m -> 1
i -> 4
s -> 4
p -> 2

m -> 1
i -> 4
s -> 4
p -> 2

1
4
4
2

m -> 1
i -> 4
s -> 4
p -> 2
```
Если обратиться к ключу, которого нет, ошибка `KeyError` не возникнет, Будет возвращено нулевое значение.


### Сравнивание 
Объекты типа `Counter` можно сравнивать между собой. Равные объекты имеют только одинаковое количество элементов и содержит равные элементы. Для сравнения используются оператор `==` и `!=`
```python
from collections import Counter

counter1 = Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
counter2 = Counter(m=1, s=4, i=4, p=2)
counter3 = Counter('iiiisspm')

print(counter1 == counter2)
print(counter1 == counter3)
```
```
True
False
```

### Подсчет объектов штатными методами
 Иногда нам нужно узнать, как часто некоторый объект встречается в каком-либо источнике данных (список, кортеж, строка и тд). Другими словами, нам нужно посчитать количество вхождений объекта. Обычно для таких целей используется переменная-счетчик, значение которой увеличивается на единицу при каждом обнаружении нужного объекта.
 ```python
data = 'mississippi'
counter = 0

for letter in data:
    if letter == 's':
        counter += 1

print(counter)
```
```
4
```
Если вам нужно посчитать количество нескольких объектов, то нам нужно создать большое количество счетчиков. Однако вместо этого куда удобнее воспользоваться одним словарем. В ключах такого словаря будут храниться объекты, которые мы считаем, а значения по этим ключам будут содержать количество повторений данного объекта.
```python
data = 'mississippi'
counter = {}

for letter in data:
     if letter not in counter:
         counter[letter] = 0
     counter[letter] += 1

print(counter)
```
```
{'m': 1, 'i': 4, 's': 4, 'p': 2}
```
Использование словаря в качестве инструмента для подсчета накладывает некоторые ограничения на ключи. Ключи должны быть хэшируемыми