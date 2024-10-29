#модуль #python #Enum #IntEnum


```python
from enum import Enum
```
### Создание перечислений
В `Python` для создания перечислений используется класс `Enum` из модуля `enum`. Чтобы создать собственное перечисление, мы можем либо создать подкласс `Enum`, либо использовать его функциональную часть. Мы рассмотрим оба способа и начнем с первого

Классическим примером использования перечислений является создание набора констант, представляющих дни недели. Каждый день недели имеет название и числовое значение от `1` до `7` включительно

Рассмотрим класс `Weekday`, представляющий данное перечисление:
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7
```
В данном примере `Weekday` - это перечисление. `Weekday.MONDAY` `Weekday.TUSDAY` и другие - это элементы перечисления. Элементы имеют имена и значения, например, именем элемента `Weekday.MONDAY` является `MONDAY`, в значением - `1`

Поскольку перечисления используются для представления констант, имена элементов перечисления рекомендуется записывать в верхнем регистре

Элементы перечисления являются функциональным константами, и представляют собой экземпляры класса перечисления
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(type(Weekday.MONDAY))
print(type(Weekday.SUNDAY))
```
```
<enum 'Weekday'>
<enum 'Weekday'>
```
Они имеют атрибуты `name` и `value`, в которых содержатся их имена и значения соответственно, также для них определены методы `__str__()` и `__repr__()` для информативного строкового представления
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(str(Weekday.MONDAY))
print(repr(Weekday.MONDAY))
print(Weekday.MONDAY.name)
print(Weekday.MONDAY.value)
```
```
Weekday.MONDAY
<Weekday.MONDAY: 1>
MONDAY
1
```
Поскольку  перечисления используются для представления констант, изменение значений их элементов невозможно, и приводит к возбуждению исключения

Зачастую элементы перечислений принимают последовательные целочисленные значения, однако они могут быть любыми и принадлежать любому типу. Например, мы можем использовать строковые значения для представления размерной сетки верхней одежды
```python
from enum import Enum

class Size(Enum):
    S = 'small'
    M = 'medium'
    L = 'large'
    XL = 'extra large'


print(Size.S)
print(Size.S.name)
print(Size.S.value)
```
```
Size.S
S
small
```

### Функция `Enum`
Для создания перечислений мы можем обойтись без использования синтаксиса создания классов, а лишь воспользоваться функциональной частью класса `Enum`, которая подразумевает вызов `Enum`  с соответствующим набором аргументов. В качестве первого аргумента мы должны указать имя класса перечисления, а в качестве второго - имена элементов перечисления. Второй аргумент может быть представлен любым итерируемым объектом, содержащим имена, или строкой, в которой имена разделены одним пробелом
```python
from enum import Enum

Weekday = Enum('Weekday', ['MONDAY', 'TUESDAY', 'WEDNESDAY', 'THURSDAY', 'FRIDAY', 'SATURDAY', 'SUNDAY'])

print(str(Weekday.MONDAY))
print(repr(Weekday.MONDAY))
print(type(Weekday.MONDAY))
print(Weekday.MONDAY.name)
print(Weekday.MONDAY.value)
```
```
Weekday.MONDAY
<Weekday.MONDAY: 1>
<enum 'Weekday'>
MONDAY
1
```
По умолчанию элементы перечисления принимают последовательные целочисленные значения, начиная с `1`. Это поведение можно изменить, передав при создании необязательный аргумент `start`, который определяет начальное значение
```python
from enum import Enum

Weekday = Enum('Weekday', ['MONDAY', 'TUESDAY', 'WEDNESDAY', 'THURSDAY', 'FRIDAY', 'SATURDAY', 'SUNDAY'], start=0)

print(repr(Weekday.MONDAY))
print(repr(Weekday.TUESDAY))
print(repr(Weekday.WEDNESDAY))
```
```
<Weekday.MONDAY: 0>
<Weekday.TUESDAY: 1>
<Weekday.WEDNESDAY: 2>
```
Также в качестве второго аргумента - имен элементов перечисления - мы можем передать итерируемый объект, в котором представлены пары имя-значение. Таким объектом может быть список кортежей или аналогичный словарь
```python
from enum import Enum

Size = Enum('Size', [('S', 'small'), ('M', 'medium'), ('L', 'large'), ('XL', 'extra large')])

print(str(Size.S))
print(repr(Size.S))
print(type(Size.S))
print(Size.S.name)
print(Size.S.value)
```
```
Size.S
<Size.S: 'small'>
<enum 'Size'>
S
small
```

### Возможности перечислений
#### Доступ к элементам 
Основной операцией при работе с перечислениями является доступ к их элементам. Выше мы уже увидели, что обращаться к элементу можно по их именам с помощью точечной нотации, однако помимо того существуют еще два способа.
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(Weekday.MONDAY)                         # точечная нотация с указанием имени элемента
print(Weekday['MONDAY'])                      # доступ по ключу в виде имени элемента
print(Weekday(1))                             # вызов перечисления с аргументом в виде значения элемента
```
```
Weekday.MONDAY
Weekday.MONDAY
Weekday.MONDAY
```
Обратите внимание, что вызов перечисления используется именно для доступа к элементам по их значениям, а не для создания экземпляров класса. Создание экземпляров класса перечисления путем вызова самого класса невозможно
#### Итерирование
Перечисления являются итерируемыми объектами, поэтому мы можем, например, перебирать их в цикле или преобразовывать в коллекции
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


for day in Weekday:
    print(day)
```
```
Weekday.MONDAY
Weekday.TUESDAY
Weekday.WEDNESDAY
Weekday.THURSDAY
Weekday.FRIDAY
Weekday.SATURDAY
Weekday.SUNDAY
```
Мы также можем обращаться к именам и значениям элементов перечисления при итерировании
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


for day in Weekday:
    print(day.name, '->', day.value)
```
```
MONDAY -> 1
TUESDAY -> 2
WEDNESDAY -> 3
THURSDAY -> 4
FRIDAY -> 5
SATURDAY -> 6
SUNDAY -> 7
```
Элементы перечисления при итерировании располагаются в том порядке, в котором они были определены в классе
#### Сравнение на равенство и идентичность
Перечисления поддерживают два вида сравнения:
- сравнение на равенство с помощью операторов `==` и `!=`
- сравнение на идентичность с помощью операторов `is` и `is not`

Каждый элемент перечисления является синглтоном, то есть существует в единственном экземпляре, поэтому при сравнении элементов удобно пользоваться более быстрыми операторами `is` и `is not`
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


day = Weekday.MONDAY

print(day is Weekday.MONDAY)
print(day is Weekday.TUESDAY)
print(day is not Weekday.MONDAY)
print(day is not Weekday.WEDNESDAY)
```
```
True
False
False
True
```
Мы также можем пользоваться операторами `==` и `!=`. Их поведение полностью повторяет поведение операторов `is` и `is not`, так как первые делегируют свои вызовы вторым
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


day = Weekday.MONDAY

print(day == Weekday.MONDAY)
print(day == Weekday.TUESDAY)
print(day != Weekday.MONDAY)
print(day != Weekday.WEDNESDAY)
```
```
True
False
False
True
```
Так как сравнение с помощью операторов `==` и `!=`, по сути, является сравнением на идентичность, результатом сравнения элементов с их фактическими значениями всегда будет `False`
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(Weekday.MONDAY == 1)
print(Weekday.TUESDAY == 2)
print(Weekday.WEDNESDAY == 3)
```
```
False
False
False
```
Так как перечисления являются итерируемыми объектами, а их элементы можно сравнивать, мы также можем пользоваться операторами проверки на принадлежность `in` и `not in`
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(Weekday.MONDAY in Weekday)
print(Weekday.TUESDAY not in Weekday)
```
```
True
False
```
#### Условные конструкции
Благодаря тому. что для перечислений доступны операции сравнения на равенство и идентичность, мы можем использовать их в условных конструкциях, делая их более наглядными и очевидными
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7

def what_to_do(day):
    if day is Weekday.SUNDAY:
        print('Сегодня выходной. Отдыхаем =)')
    else:
        print('Сегодня рабочий день. Работаем =(')


what_to_do(Weekday.MONDAY)
what_to_do(Weekday.SUNDAY)
```
```
Сегодня рабочий день. Работаем =(
Сегодня выходной. Отдыхаем =)
```
Данный подход удобен тем, что в сравнении мы используем объекты с говорящими именами, а не просто какие-либо значения
#### Сортировка
По умолчанию перечисления не поддерживают операции сравнения с помощью операторов `>`, `<`, `>=` и `<=`, поэтому для их сортировки необходимо использовать ключевую функцию
```python
from enum import Enum

class Weekday(Enum):
    SUNDAY = 7
    SATURDAY = 6
    FRIDAY = 5
    THURSDAY = 4
    WEDNESDAY = 3
    TUESDAY = 2
    MONDAY = 1


print(sorted(Weekday, key=lambda day: day.value))
```
```
[<Weekday.MONDAY: 1>, <Weekday.TUESDAY: 2>, <Weekday.WEDNESDAY: 3>, <Weekday.THURSDAY: 4>, <Weekday.FRIDAY: 5>, <Weekday.SATURDAY: 6>, <Weekday.SUNDAY: 7>]
```
#### Пользовательские методы
Лишь создав класс перечисления и определив в нем набор элементов, мы уже получаем неплохой стартовый функционал, однако по желанию он может быть изменен и расширен
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7

    def about(self):                      # метод экземпляра, возвращающий данные элемента
        return self.name, self.value
 
    def __str__(self):                    # переопределенный магический метод __str__()
        return f'Today is {self.name}'    

    @classmethod              
    def favorite_day(cls):                # метод класса, возращающий элемент Weekday.SUNDAY
        return cls.SUNDAY


day = Weekday.favorite_day()

print(day)
print(day.about())
```
```
Today is SUNDAY
('SUNDAY', 7)
```
Аналогичным образом мы можем определить методы сравнения элементов перечисления, тем самым добавив возможность их сортировки

Не стоит забывать, что элементы перечисления являются экземплярами класса перечисления

### Примечания
##### Примечание 1
Официальная документация по модулю `enum` доступна по [ссылке](https://docs.python.org/3/library/enum.html#module-enum).
##### Примечание 2
 Все перечисления (наследники класса `Enum`) имеют следующие магические методы:
- `__contains__()`
- `__getitem__()`
- `__iter__()`
- `__len__()`
- `__reversed__()`

```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(Weekday.SUNDAY in Weekday)
print(Weekday['SUNDAY'])
print(list(Weekday))
print(len(Weekday))
print(list(reversed(Weekday)))
```
```
True
Weekday.SUNDAY
[<Weekday.MONDAY: 1>, <Weekday.TUESDAY: 2>, <Weekday.WEDNESDAY: 3>, <Weekday.THURSDAY: 4>, <Weekday.FRIDAY: 5>, <Weekday.SATURDAY: 6>, <Weekday.SUNDAY: 7>]
7
[<Weekday.SUNDAY: 7>, <Weekday.SATURDAY: 6>, <Weekday.FRIDAY: 5>, <Weekday.THURSDAY: 4>, <Weekday.WEDNESDAY: 3>, <Weekday.TUESDAY: 2>, <Weekday.MONDAY: 1>]
```
##### Примечание 3
Чаще всего значениями элементов перечислений являются целые числа, поэтому в модуле `enum` помимо класса `Enum` имеется класс `IntEnum`. Наследуясь от данного класса, можно наделить элементы перечисления всем функционалом целых чисел, а именно возможностью выполнять с ними арифметические операции, а также производить всевозможные сравнения.
```python
from enum import IntEnum

class Weekday(IntEnum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(Weekday.MONDAY < Weekday.SUNDAY)
print(Weekday.THURSDAY > Weekday.MONDAY)
print(Weekday.MONDAY + Weekday.SUNDAY)
print(Weekday.SUNDAY - Weekday.MONDAY)
print(Weekday.TUESDAY * Weekday.THURSDAY)
```
```
True
True
8
6
8
```
При определении перечисления, наследуемого от `IntEnum`, значения элементов должны быть такими, чтобы их можно было преобразовать в целые числа, в ином случае будет возбуждено исключение.
```python
from enum import IntEnum

class Weekday(IntEnum):
    MONDAY = 'a'
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7
```
```
ValueError: invalid literal for int() with base 10: 'a'
```
В Python 3.11 также был добавлен класс `StrEnum`. Подробнее о нем можно почитать по [ссылке](https://docs.python.org/3/library/enum.html#enum.StrEnum).
##### Примечание 4
Элементы перечисления, имеющие одинаковые значения, являются одним и тем же объектом, причем тем, который был создан раньше. На этот объект ссылаются все элементы перечисления, имеющие то же значение, при этом имя может быть любым.
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7
    Monday = 1


print(Weekday.MONDAY)
print(Weekday.Monday)
print(Weekday.MONDAY is Weekday.Monday)
```
```
Weekday.MONDAY
Weekday.MONDAY
True
```
Элементы с одинаковыми значениями, созданные позже, не учитываются при итерировании по перечислению.
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7
    Monday = 1


for day in Weekday:
    print(day.name, '->', day.value)
```
```
MONDAY -> 1
TUESDAY -> 2
WEDNESDAY -> 3
THURSDAY -> 4
FRIDAY -> 5
SATURDAY -> 6
SUNDAY -> 7
```
Если мы хотим, чтобы все элементы перечисления обязательно имели разные значения, то мы можем воспользоваться декоратором `@unique` из модуля `enum`. Данный декоратор приведет к возбуждению исключения, если в перечислении значения каких-либо элементов совпадают.
```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7
    Monday = 1
```
```
ValueError: duplicate values found in <enum 'Weekday'>: Monday -> MONDAY
```
##### Примечание 5
Элементы перечисления являются хешируемыми, то есть мы можем использовать их в качестве ключей в словарях, а также в качестве элементов в множествах.
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3
    THURSDAY = 4
    FRIDAY = 5
    SATURDAY = 6
    SUNDAY = 7


print(set(Weekday))
```
```
{<Weekday.THURSDAY: 4>, <Weekday.SATURDAY: 6>, <Weekday.TUESDAY: 2>, <Weekday.MONDAY: 1>, <Weekday.WEDNESDAY: 3>, <Weekday.FRIDAY: 5>, <Weekday.SUNDAY: 7>}
```
##### Примечание 6
 Если значениями элементов перечисления являются последовательные целые числа, начиная с `1`, мы можем не прописывать их явно, а воспользоваться функцией `auto()` из модуля `enum`.
```python
from enum import Enum, auto

class Weekday(Enum):
    MONDAY = auto()
    TUESDAY = auto()
    WEDNESDAY = auto()
    THURSDAY = auto()
    FRIDAY = auto()
    SATURDAY = auto()
    SUNDAY = auto()


for day in Weekday:
    print(day.name, '->', day.value)
```
```
MONDAY -> 1
TUESDAY -> 2
WEDNESDAY -> 3
THURSDAY -> 4
FRIDAY -> 5
SATURDAY -> 6
SUNDAY -> 7
```