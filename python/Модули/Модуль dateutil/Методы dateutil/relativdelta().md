#метод #datetime #python #dateutil #date


Метод для работы с объектами `datetime`  и `date`, может изменять определенные компоненты этих объектов или представлять из себя разницу между ними. В отличии от `timedelta` умеет работать с месяцами и годами и днями недели
```python
relativedaelta(datetime1, datetime2)
```
В этом случае метод вернет разницу между двумя датами

А можно собирать их руками
```python
relativedaelta(years, months, days, hours, minutes, seconds, microseconds, weekday)
```

Еще можно к значениям в объектах `datetime/date` прибавлять время при помощи `relativdelta`, а можно вообще заменять:
```python
from datetime import datetime, date
from dateutil.relativedelta import relativedelta

dt_now = datetime.now()
print("Дата и время прямо сейчас:", dt_now)
dt_today = date.today()
print("Дата сегодня:", dt_today)

# Следующий месяц
print(dt_now + relativedelta(months=+1))
# Следующий месяц, плюс одна неделя
print(dt_now + relativedelta(months=+1, weeks=+1)) 
# Следующий месяц, плюс одна неделя, в 17:00.
print(dt_now + relativedelta(months=+1, weeks=+1, hour=17))
# Следующая пятница
print(dt_today + relativedelta(weekday=4))
```
```
2021-03-06 12:11:53.648565 
2021-03-13 12:11:53.648565 
2021-03-13 17:11:53.648565 
2021-02-12
```

Еще эта штука умеет работать с днями недели, но их нужно заранее импортировать
```python
from dateutil.relativedelta import relativedelta, MO, TU, WE, TH, FR, SA, SU
from datetime import datetime

# Задаем начальную дату
dt = datetime(2024, 5, 8)  # 8 мая 2024 года

# Получаем дату следующего вторника после заданной даты
next_tuesday = dt + relativedelta(weekday=TU(1))

print(next_tuesday)  # Выведет дату первого вторника после 8 мая 2024 года
```
А `TU(-1)` вернет предыдущий вторник, `TU(2)` вернет второй вторник из будущего и тд.

Есть еще относительно полезные методы:
`datetime()` вернет текущую дату и время
`today()` вернет текущую дату
```python
# нужно импортировать модуль datetime
from datetime import datetime, date
from dateutil.relativedelta import relativedelta, MO, TU, WE, TH, FR, SA, SU

# Создание нескольких объектов datetime для работы
dt_now = datetime.now()
print("Дата и время прямо сейчас:", dt_now)
dt_today = date.today()
print("Дата сегодня:", dt_today)
```
```
Дата и время прямо сейчас: 2021-02-06 12:06:58.192574 
Дата сегодня: 2021-02-06
```