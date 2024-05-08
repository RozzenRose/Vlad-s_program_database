Модуль #datetime #неизменяемый #объект #тип_данных #python #tzinfo


Абстрактны класс для работы с информацией о часовом поясе

```python
from datetime import datetime, timedelta, tzinfo

class SimpleUTC(tzinfo):
    def utcoffset(self, dt):
        return timedelta(0)
    def tzname(self, dt):
        return "UTC"
    def dst(self, dt):
        return timedelta(0)

utc = SimpleUTC()
dt = datetime(2024, 5, 8, 17, 15, 1, tzinfo=utc)
print(dt)  # Выведет '2024-05-08 17:15:01+00:00'
```
Объекты, которые являются подклассами `tzinfo`, могут быть связаны с объектами `datetime`, чтобы предоставить информацию о часовом поясе.

Вот основные методы, которые должны быть реализованы в подклассах `tzinfo`: 

`utcoffset(self, dt)`: Возвращает смещение времени от UTC для переданного объекта 
`datetime`. Смещение обычно выражается в минутах или `timedelta`.

`dst(self, dt)`: Возвращает продолжительность периода летнего времени (Daylight Saving Time) для переданного объекта `datetime`. Также возвращается в виде `timedelta`.

`tzname(self, dt)`: Возвращает строковое название часового пояса для переданного объекта `datetime`.

`fromutc(self, dt)`: Метод для конвертации времени в UTC в местное время.

Объекты `datetime` могут быть “наивными” (без информации о часовом поясе) или “осведомленными” (с информацией о часовом поясе). [Если объект `datetime` связан с объектом `tzinfo`, он считается “осведомленным” и может учитывать различия в часовых поясах и переход на летнее время](https://www.geeksforgeeks.org/python-datetime-tzinfo/)[1](https://www.geeksforgeeks.org/python-datetime-tzinfo/).


Вообще пытаться собрать такое руками - плохая идея лучше использовать подкласс tz из модуля [[Модуль dateutil|dateutil]]