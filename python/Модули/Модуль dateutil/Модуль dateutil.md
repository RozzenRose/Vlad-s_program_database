#модуль #datetime #python #dateutil #relativedate #date


Этот модуль содержит более продвинутые методы, для работы с с типом данных `datetime` из модуля [[python/Модули/Модуль datetime/Модуль datetime|datetime]]

Перед использованием, его нужно накатить:
```cmd
pip install python-dateutil
```
Для Linux удобнее так:
```Terminal
sudo pacman -S python-dateutil
```

Модуль разбит на подклассы
## Подклассы:
### relativdelta (как timedelta, но круче)
Содержит объект [[relativdelta()]]
```python
from dateutil.relativedelta import relativedelta, MO, TU, WE, TH, FR, SA, SU
```
### easter Христос Воскрес
Содержит метод, который, возвращает объект `date`, содержащий дату Пасхи указанного года
```python
from dateutil.easter import easter
western_easter_2024 = easter(2024, method=3)
print(western_easter_2024)
```
```
2024-03-31
```
### rapser строка в datetime
Содержит метод [[parser()]]
```python
from dateutil.parser import parse
```
### rrule цикличный набор дат
```python
from dateutil import rrule
```
Содержит метод [[rrule() rruleset()]] для работы с повторяющимися событиями
### tz таймзоны:
[[tzinfo]] - это класс из модуля `datetime`
#### tzutc делает tzinfo в UTC
Это объект `tzinfo`, представляющий часовой пояс UTC
```python
from dateutil.tz import tzutc
from datetime import datetime

utc_time = datetime.now(tzutc())
print(utc_time)  # Выведет текущее время в UTC
```
#### tzoffset создает свой часовой пояс UTC
Создает `tzinfo` фиксировано смещенного часового пояса от UTC
```python
from dateutil.tz import tzoffset
from datetime import datetime, timedelta

offset_time = datetime(2024, 5, 8, 17, 15, 1, tzinfo=tzoffset("EST", -18000))
print(offset_time)  # Выведет время с учетом смещения EST
```
`name` - название создаваемого часового пояса
`offset` - смещение в секундах от UTC
#### tzlocal локальный UTC
Получает `tzinfo` локальной машины
```python
from dateutil.tz import tzlocal
from datetime import datetime

local_time = datetime.now(tzlocal())
print(local_time)  # Выведет локальное время машины
```
#### gettz получает объект tzinfo из строки
```python
from dateutil.tz import gettz
from datetime import datetime

ny_tz = gettz('America/New_York')
ny_time = datetime.now(ny_tz)
print(ny_time)  # Выведет время в часовом поясе Нью-Йорка
```
`name` - название часового пояса, путь к файлу `tzfile` или строка в стиле переменной среды TZ
#### tzfile файлы часовых поясов
Класс для работы с файлами часовых поясов в формате `ttzfile`
```python
from dateutil.tz import tzfile
from datetime import datetime

tz = tzfile('/usr/share/zoneinfo/America/New_York')
ny_time = datetime.now(tz)
print(ny_time)  # Выведет время в часовом поясе Нью-Йорка
```

#### tzstr работа со строками с поясами в стиле GNU TZ
```python
from dateutil.tz import tzstr
from datetime import datetime

tz = tzstr('EST+5EDT,M3.2.0/2,M11.1.0/2')
est_time = datetime.now(tz)
print(est_time)  # Выведет время в часовом поясе EST с учетом перехода на летнее время
```