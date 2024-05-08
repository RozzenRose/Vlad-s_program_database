Модуль #timedelta #неизменяемый #объект #тип_данных #python 


Представляет собой временной интервал, который выражает разницу между двумя объектами типов `time` или `datetime`. Используется для различных манипуляций.

При создании объекта `timedelta` можно указать следующие аргументы:
- недели (weeks)
- дни (days)
- часы (hours)
- минуты (minutes)
- секунды (seconds)
- миллисекунды (milliseconds)
- микросекунды (microseconds)

Можно использовать любые их сочетания, при этом любые аргументы не являются обязательными и по умолчанию равны 0

Задаются аргументы `int`ами или `float`ами даже отрицательными
```python
from datetime import timedelta

delta = timedelta(days=7, hours=20, minutes=7, seconds=17)

print(delta)
print(type(delta))
```
```
7 days, 20:07:17
<class 'datetime.timedelta'>
```
`timedelta` на самом деле хранит только сочетание `days, seconds, microseconds`, все остальное при передаче в этот объект конвертируется в эти единицы.
`timedelta` может быть отрицательным
```python
from datetime import timedelta

delta1 = timedelta(minutes=-40)
delta2 = timedelta(seconds=-10, weeks=-2)

print(delta1)
print(delta2)
```
```
-1 day, 23:20:00
-15 days, 23:59:50
```

Есть атрибуты, которые позволяют, вытащить из объекта количество конкретных временных единиц
```python
from datetime import timedelta

delta = timedelta(days=50, seconds=27, microseconds=10, milliseconds=29000, minutes=5, hours=8, weeks=2)

print('Количество дней =', delta.days)
print('Количество секунд =', delta.seconds)
print('Количество микросекунд =', delta.microseconds)
print('Общее количество секунд =', delta.total_seconds())
```
```
Количество дней = 64
Количество секунд = 29156
Количество микросекунд = 10
Общее количество секунд = 5558756.00001
```
Метод `total_seconds()` вернет вообще всю временную величину объекта в секундах.

Вытащить количество часов и минут при помощи атрибутов невозможно, так что если они нужны придется вычислить их самостоятельно из того, что все таки можно вытащить при помощи аргументов
```python
def hours_mimute(td):
	return td.seconds // 3600, (td.seconds // 60) % 60
```

Объекты можно сравнивать операторами `==, !- , <, >, <=, >=`:
```python
from datetime import timedelta

delta1 = timedelta(weeks=1)
delta2 = timedelta(hours=24*7)
delta3 = timedelta(minutes=24*7*59)

print(delta1 == delta2)
print(delta1 != delta3)
print(delta1 < delta3)
```
```
True
True
False
```
При сравнении с любыми другими типами объектами возникнет исключение `TypeError` даже с `int`ами или `float`ами 
```python
from datetime import timedelta

delta1 = timedelta(seconds=57)
delta2 = timedelta(hours=25, seconds=2)

print(delta2 > delta1)     # тут все ок
print(delta2 > 5)
```
```
TypeError: '>' not supported between instances of 'datetime.timedelta' and 'int'
```

Что вообще с этими объектами можно делать:
- сложение временных интервалов
- вычитание временных интервалов
- умножение временного интервала на число
- деление временного интервала на число
- деление временного интервала на другой временной интервал

Можно прибавлять к объектам типов `datetime` или `date`
```python
from datetime import datetime, date, timedelta

delta1 = datetime(2021, 1, 1, 12, 15, 20) - datetime(2020, 5, 1, 10, 5, 10)
delta2 = date(2020, 2, 29) - date(2019, 9, 1)
delta3 = date(2019, 9, 1) - date(2020, 2, 29)

print(delta1)
print(delta2)
print(delta3)
```
```
245 days, 2:10:10
181 days, 0:00:00
-181 days, 0:00:00
```

#### str() repr()
Мы можем использовать `str()` для преобразования объектов `timedelta` в строковый тип и `repr()`для преобразования в строку с сохранением формального представления, как в коде
```python
from datetime import timedelta

delta1 = timedelta(weeks=1, hours=23, minutes=61)
delta2 = timedelta(minutes=-300)

print(str(delta1), str(delta2), sep='\n')
print(repr(delta1), repr(delta2), sep='\n')
```
```
8 days, 0:01:00
-1 day, 19:00:00
datetime.timedelta(days=8, seconds=60)
datetime.timedelta(days=-1, seconds=68400)
```

#### abs()
К обьекетам типа `timedate` можно применять метод `abs()`, у них есть модуль