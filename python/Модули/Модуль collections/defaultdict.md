#collections #Python #defaultdict #объект


Основная проблема при работе с штатными словарями `dict` - это попытка получить данные по несуществующему ключу. Это приводит к ошибке `KeyError`. Несмотря на то что существуют штатные способы решения этой проблемы, стандартная библиотека предоставляет тип данных `defaultdict`. 

Тип `defaultdict` ведет себя почти так же, как обычный словарь `dict`, но если мы попытаемся получить доступ по несуществующему ключу, то `defaultdict` автоматически создаст ключ и сгенерирует для него значение по умолчанию. Такое поведение делает `defaultdict` удобным вариантом обработки недостающих ключей в словарях.

```python
info = {'name': 'Vladick', 'age': 29, 'job': 'Backender'}

print(info['salary'])
```
```
KeyError: 'salary'
```

Существует несколько способов с помощью которых можно избежать возникновения такой ошибки:
- метод `setdefault()`
- метод `get()`
- проверка наличия ключа с помощью оператора принадлежности (`key in dict`)

Ну или можно просто использовать альтернативную версию словаря `defaultdict`
```python
from collections import defaultdict

info = defaultdict(int)       # создаем словарь со значением по умолчанию 0

info['name'] = 'Vladick'
info['age'] = 29
info['job'] = 'ASUDev'

print(info['salary'])
print(info)
```
```
0
defaultdict(<class 'int'>, {'name': 'Vladick', 'age': 29, 'job': 'ASUDev', 'salary': 0})
```

```python
from collections import defaultdict
info = defaultdict(defolt_type, found_dict)
```
Метод `defaultdict()` принимает на вход тип объекта, который будет являться типом объекта по дефолту.  После этого, в случаях, когда у `defaultdict` будут запрашиваться данные по несуществующему ключу, по запрошенному ключу будет создаваться элемент, содержащий объект типа, который был указан в качеству дефолтного, при этом будет содержать дефолтное значение:
- для `int` – число `0`
- для `float` – число `0.0`
- для `bool` – значение `False`
- для `str` – пустая строка `''`
- для `list` – пустой список `[]`
- для `tuple` – пустой кортеж `()`
- для `set` – пустое множество `set()`
- для `dict` – пустой словарь `{}`

Но вообще, можно туда и значение дефолтное передать, не только тип, можно как бы написать туда цифру или строку. Можно даже функцию туда прописать, которая возвращает будет возвращать какое-нибудь значение.

В качестве второго аргумента в метод можно передать аргумент, на базе которого будет создан экземпляр `defaultdict`
```python
from collections import defaultdict

info = defaultdict(int, {'name': 'Vlaldick', 'age': 29, 'job': 'ASUDev'})

print(info['name'])
print(info['salary'])
print(info)
```
```
Timur
0
defaultdict(<class 'int'>, {'name': 'Vladick', 'age': 29, 'job': 'ASUDev', 'salary': 0})
```
Для создания таких объектов доступны все те же самые инструменты, которые используются для создания обычных объектов типа `dict`, а именно передача именованных аргументов или итерируемого объекта, содержащего пары ключ-значение(например, список кортежей).
```python
from collections import defaultdict

info1 = defaultdict(int, name='Vlad', age=29, job='ASUDev')
info2 = defaultdict(int, [('name', 'Vlad'), ('age', 29), ('job', 'ASUDev')])

print(info1)
print(info2)
```
```
defaultdict(<class 'int'>, {'name': 'Vlad', 'age': 29, 'job': 'ASUDev'})
defaultdict(<class 'int'>, {'name': 'Vlad', 'age': 29, 'job': 'ASUDev'})
```

Забавно, что при создании словаря `defaultdict` мы можем указать только именованные аргументы без дефолтного типа, но не можем указать только итерируемый объект с парами ключ-значение без дефолтного типа:
```python
from collections import defaultdict

info = defaultdict(name='Timur', age=29, job='Teacher')

print(info)
```
```
defaultdict(None, {'name': 'Timur', 'age': 29, 'job': 'Teacher'})
```
Все нормально как бы прошло.

```python
from collections import defaultdict

info = defaultdict([('name', 'Timur'), ('age', 29), ('job', 'Teacher')])

print(info)
```
А вот такой код приведет уже к возникновению ошибки. 

Тип `defaultdict` наследуется от типа `dict`:
```python
from collections import defaultdict

print(issubclass(defaultdict, dict))
```
```
True
```
Таким образом, все методы доступные для обычных `dict`, также доступны и для `defaultdict` словарей. Их можно даже сравнивать:
```python
from collections import defaultdict

info1 = {'name': 'Vladick', 'age': 29, 'job': 'ASUDev'}
info2 = defaultdict(int, {'name': 'Vladick', 'age': 29, 'job': 'ASUDev'})

print(info1 == info2)
```
```
True
```

Можно менять дефолтное значение на ходу через атрибут `default_factory`:
```python
from collections import defaultdict

data = defaultdict(int)
print(data['salary1'])

data.default_factory = list
print(data['salary2'])

data.default_factory = float
print(data['salary3'])
```
```
0
[]
0.0
```

### Примеры на конкретных задачах:
 пусть задан список чисел `numbers`, в котором некоторые числа встречаются несколько раз. Нужно узнать, сколько именно раз встречается каждое из чисел.

**Решение 1.** С помощью метода `setdefault()`:
```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    value = result.setdefault(num, 0)
    result[num] = value + 1
```

**Решение 2.** С помощью метода `get()`:
```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    result[num] = result.get(num, 0) + 1
```

**Решение 3.** С помощью оператора принадлежности `in`:
```python
numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]

result = {}
for num in numbers:
    if num not in result:
        result[num] = 1
    else:
        result[num] += 1
```

**Решение 4.** С помощью типа данных `defaultdict`:

Эта работает быстрее, чем решение задачи через методы `setdefault()` или `get()`
```python
from collections import defaultdict

numbers = [9, 8, 32, 1, 10, 1, 10, 23, 1, 4, 10, 4, 2, 2, 2, 2, 1, 10, 1, 2, 2, 32, 23, 23]
result = defaultdict(int)

for num in numbers:
    result[num] += 1
```

Тип данных `defaultdict` часто используют в связке с пустым списком в качестве значения по умолчанию, чтобы начинать добавление элементов без лишнего кода.

```python
from collections import defaultdict

my_dict = defaultdict(list)

for i in range(7):
    my_dict[i].append(i)

for key in my_dict:
    print(key, my_dict[key])
```
```
0 [0]
1 [1]
2 [2]
3 [3]
4 [4]
5 [5]
6 [6]
```
При использовании `defaultdict` нет необходимости ни проверять наличие соответствующих ключей в словаре, ни создавать предварительно пустые списки.

Статья про `default`: https://realpython.com/python-defaultdict/