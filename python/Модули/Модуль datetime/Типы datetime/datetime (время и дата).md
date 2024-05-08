Модуль #datetime #неизменяемый #объект #тип_данных #python 


В [[[Модуль datetime|модуле datetime]] есть тип объектов `datetime`, просто ахуеть.
Работает как типы `date` и `time`, но одновременно. Сразу и с датой и с временем. Я до сих пор в ахуе!!

Для использования требует импортирования из модуля `datetime`
```python
from datetime import datetime

my_datetime = datetime(1992, 10, 6, 9, 40, 23, 51204)
only_date = datetime(2021, 12, 31)

print(my_datetime)
print(only_date)
print(type(my_datetime))
```
```
1992-10-06 09:40:23:23.051204
2021-12-31 00:00:00
<class 'datetime.datetime'>
```
год => месяц => день => часы => минуты => секунды => микросекунды
Даля создания класса обязательно должны быть указаны первые 3 цифры (год, месяц, день)

Можно прописать свой порядок через атрибуты:
```python
datetime(day=6, month=10, year=1992, second=23, minute=40, microsecond=51204, hour=9)
```

Можно создать свой формат данных через [[strptime()]] или [[strptime()]]
Через `strftime()` мы в соответствии с определенным форматом можем перевести данные в `str
Через `strptime()` мы можем `str` в определенном формате перевести в `strptime()`
### combine() date + time = datetime
Позволяет из объектв типов `date` и `time` собрать один объект типа `datetime`
```python
from datetime import date, time, datetime

my_date = date(1992, 10, 6)
my_time = time(10, 42, 17)
my_datetime = datetime.combine(my_date, ,my_time)

print(my_datetime)
```
```
1992-10-06 10:45:17
```
 Работает еще и наоборот

### date() time() разобрать datetime
Из объекта класса класса `datetime` позволяет полкчить объекты типов `date` и `time`
```python
from datetime import datetime

my_datetime = datetime(2022, 10, 7, 14, 15, 45)
my_date = my_datetime.date()
my_time = my_datetime.time()

print(my_datetiem, type(my_datetiem))
print(my_date, type(my_date))
print(my_time, type(my_time))
```
```
2022-10-07 14:15:45 <class 'datetime.datetime'>
2022-10-07 <class 'datetime.date'>
14:15:45 <class 'datetime.time'>
```

### now() utcnow() today() время и дата сейчас
Используется для получения текущего времени
```python
from datetime import datetime

datetime_now = datetime.now()
datetime_utcnow = datetime.utcnow()
datetime_today = datetime.today()

print(datetime_now)
print(datetime_utcnow)
print(datetime_today)
```
```
2024-05-03 21:48:25.887074
2024-05-03 19:48:25.887079
2024-05-03 21:48:25.887084
```
Прикол в том, что `now()` и `today()` делают вообще одно и тоже, но выводят время по местному формату с соответствующем смещением по UTC.
А вот метод `utcnow()` возвращет значение, которое соответствует UTC.

### timestamp() в секундах с начала эпохи
Преобразовывает объект типа `datetime` в объект типа `float` который будет содержать количество секунд с начала эпохи Unix (1 января 1970г 00:00:00 UTC)
```python
from datetime import datetime

my_datetime = datetime(2021, 10, 13, 8, 10, 23)

print(my_datetime.timestamp())
```
```
1634101823.0
```

### fromtimestamp() из секунд с начала эпохи
Метод обратный методу `timestamp()`. Возьмет значение из объекта типа `float`, представит его, как количество секунд с начала эпохи и сформирует объект типа `datetime`. (Если закинуть туда `int()`, тоже работает)
```python
from datetime import datetime

my_datetime = datetime.fromtimestamp(1634101823.0)

print(my_datetime)
```
```
2021-10-13 08:10:23
```



### Наивность и осведомленность

Объекты типа `datetime` могут быть наивными и осведомленными.
Наивные не знают о том, какому часовому поясу они относятся.
Осведомленные знают.

По умолчанию все объекты класса `datetime` наивные, а временные смещения применяются к ней системные. 

Разница в том содержит `datetime` `tzinfo` или нет

Наивный
```python
from datetime import datetime

# Наивный объект datetime
naive_dt = datetime(2024, 5, 8, 17, 15, 1)
print(naive_dt)  # Выведет '2024-05-08 17:15:01'
```
```
2024-05-08 17:15:01
```
Осведомленный
```python
from datetime import datetime
from dateutil.tz import gettz

# Осведомленный объект datetime с часовым поясом
aware_dt = datetime(2024, 5, 8, 17, 15, 1, tzinfo=gettz('Europe/Belgrade'))
print(aware_dt)  # Выведет '2024-05-08 17:15:01+02:00'
```
```
2024-05-08 17:15:01+02:00
```