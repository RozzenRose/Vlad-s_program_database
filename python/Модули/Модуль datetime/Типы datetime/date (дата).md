Модуль #datetime #неизменяемый #объект #тип_данных #python 


Тип данных из [[Модуль datetime|модуля datetime]]
Естественно требует подкинуть этот модуль

Тип данных, который используется для представления данных о дате, включает, `год`, `месяц` и `день`.
```python
from datetime import date

my_date = date(1995, 12, 20) # тип данных date: год + месяц + день

print(my_date)
print(type(my_date))
```
```
1995-12-20
<class 'datetime.date'>
```
Тип `date` имеет атрибуты `year`, `month` и `day`
```python
from datetime import date

my_date = date(day=20, month=12, year=1995)

print('Год =', my_date.year)
print('Месяц =', my_date.month)
print('День =', my_date.day)
```
```
Год = 1995
Месяц = 12
День = 20
```
У этого типа объектов есть минимальные и максимальные значения:
```python
from datetime import date

print(date.min)
print(date.max)
```
```
0001-01-01
9999-12-31
```
Еще существуют атрибуты, которые передают минимальное и максимальное значения `MINYEAR` и `MAXYEAR`.
```python
import datetime
print(datetime.MAXYEAR)
print(datetime.MINYEAR)
```
```
9999
1
```
Существуют ограничения для каждого числа

| Годы   | 0001 <= `hour` <= 9999 |
| ------ | ---------------------- |
| Месяцы | 01 <= `minute` <= 12   |
| Дни    | 01 <= `second` <= 31   |

Пример преобразования из  строки:

```python
from datetime import date, time

day, month, year = input('Введите дату в формате ДД.ММ.ГГГГ').split('.')
hour, minute, second = input('Введите время в формате ЧЧ:ММ:СС').split(':')

my_date = date(int(year), int(month), int(day)) # создаем объект типа date
my_time = time(int(hour), int(minute), int(second)) # создаем объект типа time

print(my_date)
print(my_time)
```
```
2021-11-13
21:34:59
```
### today()
Можно получить сегодняшнее число при помощи метода `today()`
```python
from datetime import date

creation_date = date.today()
print(cration_date)
```
```
2024-05-02
```
### weekday()
Так же можно определить день недели при помощи метода `weekday()`, ему передается объект типа `date`, он возвращает цифру, которая и обозначает день недели:
- 0 = понедельник
- 1 = вторник
- 2 = среда
- 3 = четверг
- 4 = пятница
- 5 = суббота
- 6 = воскресенье
```python
from datetime import date

date1 = date(2022, 10, 15)
print(date1.weekday())
```
```
5 # суббота
```
### isoweekday()
Делает все то же самое, что и метод `weekday()`, но нумерация дней недели располагается от 1 до 7. Да, вот для этого сделали отдельный метод, живи теперь с этим.
```python
from datetime import date

date1 = date(2022, 19, 15)
print(date1.isoweekday())
```
```
6 # суббота
```
### fromordinal() и toordinal()
Первый позволяет собрать объект типа `date` из номера дня , начиная с `0001-01-01` представленного в объекте типа `int`

А второй делает все наоборот, из объекта типа `date`, получает объект типа `int`, который отображает количество дней, начиная с `0001-01-01`
```python
from datatime import date

date1 = date.fromordinal(365)
date2 = date.(1999, 12, 26)

print(date1)
print(date2.toordinal())
```
```
0001-12-31
730114
```