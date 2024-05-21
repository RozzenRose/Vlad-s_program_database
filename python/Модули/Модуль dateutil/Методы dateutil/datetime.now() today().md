#datetime #python #dateutil #now #today #date #метод


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
