Модуль #datetime #неизменяемый #объект #тип_данных


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

### strptime() строка в datetime
Преобразовывать строку в `datetime` руками через метод `split()` довольно неудобно и нужно нагородить кучу кода. `strptime()` позволяет сделать это дольно удобно.
```python
from datetime import datetime 
datetime0 = datetime.strptime('10.08.2034 13:55:59', '%d.%m.%Y %H:%M:%S')
datetime1 = datetime.strptime('10/08/21', '%d/%m/%y') 
datetime2 = datetime.strptime('Tuesday 10, August 2021', '%A %d, %B %Y') 
datetime3 = datetime.strptime('18.20.34', '%H.%M.%S') 
datetime4 = datetime.strptime('2021/08/10', '%Y/%m/%d') 
datetime5 = datetime.strptime('10.08.2021 (Tuesday, August)', '%d.%m.%Y (%A, %B)') datetime6 = datetime.strptime('Year: 2021, Month: 08, Day: 10, Hour: 18.', 'Year: %Y, Month: %m, Day: %d, Hour: %H.') 

print(datetime0, datetime1, datetime2, dateti
```
```
2034-08-10 13:55:59 
2021-08-10 00:00:00 
2021-08-10 00:00:00 
1900-01-01 18:20:34 
2021-08-10 00:00:00 
2021-08-10 00:00:00 
2021-08-10 18:00:00
```
Табличка такая же ка для метода `strftime()`

| Формат | Значение                                                                                                                   | Пример                                                                                                             |
| ------ | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `%a`   | Сокращенное название дня недели                                                                                            | Sun, Mon, …, Sat (en_US)  <br>Пн, Вт, ..., Вс (ru_RU)                                                              |
| `%A`   | Полное название дня недели                                                                                                 | Sunday, Monday, …, Saturday (en_US)  <br>понедельник, ..., воскресенье (ru_RU)                                     |
| `%w`   | Номер дня недели [0, …, 6]                                                                                                 | 0, 1, …, 6 (0=воскресенье, 6=суббота)                                                                              |
| `%d`   | День месяца [01, …, 31]                                                                                                    | 01, 02, …, 31                                                                                                      |
| `%b`   | Сокращенное название месяца                                                                                                | Jan, Feb, …, Dec (en_US);  <br>янв, ..., дек (ru_RU)                                                               |
| `%B`   | Полное название месяца                                                                                                     | January, February, …, December (en_US);  <br>Январь, ..., Декабрь (ru_RU)                                          |
| `%m`   | Номер месяца [01, …,12]                                                                                                    | 01, 02, …, 12                                                                                                      |
| `%y`   | Год без века [00, …, 99]                                                                                                   | 00, 01, …, 99                                                                                                      |
| `%Y`   | Год с веком                                                                                                                | 0001, 0002, …, 2013, 2014, …, 9999  <br>В Linux год выводится без ведущих нулей:  <br>1, 2, …, 2013, 2014, …, 9999 |
| `%H`   | Час (24-часовой формат) [00, …, 23]                                                                                        | 00, 01, …, 23                                                                                                      |
| `%I`   | Час (12-часовой формат) [01, …, 12]                                                                                        | 01, 02, …, 12                                                                                                      |
| `%p`   | До полудня или после (при 12-часовом формате)                                                                              | AM, PM (en_US)                                                                                                     |
| `%M`   | Число минут [00, …, 59]                                                                                                    | 00, 01, …, 59                                                                                                      |
| `%S`   | Число секунд [00, …, 59]                                                                                                   | 00, 01, …, 59                                                                                                      |
| `%f`   | Число микросекунд                                                                                                          | 000000, 000001, …, 999999                                                                                          |
| `%z`   | Разница с UTC в формате ±HHMM[SS[.ffffff]]                                                                                 | +0000, -0400, +1030, +063415, ...                                                                                  |
| `%Z`   | Временная зона                                                                                                             | UTC, EST, CST                                                                                                      |
| `%j`   | День года [001,366]                                                                                                        | 001, 002, …, 366                                                                                                   |
| `%U`   | Номер недели в году (неделя начинается с воскр.).Неделя, предшествующая первому воскресенью, является нулевой. [00, …, 53] | 00, 01, …, 53                                                                                                      |
| `%W`   | Номер недели в году (неделя начинается с пон.) Неделя, предшествующая первому понедельнику, является нулевой. [00, …, 53]  | 00, 01, …, 53                                                                                                      |
| `%c`   | Дата и время                                                                                                               | Tue Aug 16 21:30:00 1988 (en_US);  <br>03.01.2019 23:18:32 (ru_RU)                                                 |
| `%x`   | Дата                                                                                                                       | 08/16/88 (None); 08/16/1988 (en_US);  <br>03.01.2019 (ru_RU)                                                       |
| `%X`   | Время                                                                                                                      | 21:30:00                                                                                                           |
