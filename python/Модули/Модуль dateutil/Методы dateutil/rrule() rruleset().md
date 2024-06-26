#метод #datetime #python #dateutil #rrule


Метод предназначен для работы с повторяющимися событиями
Метод `rrule` возвращает итератор, который содержит объекты типа `datetime`. Этот итератор формируется в зависимости от того, какие параметры были переданы методу.
```python
rule = list(rrule(freq=, dtstart=, interval=, count=, wkst=, until=))
```
Основные параметры:
- `freq`: Частота повторения, которая может быть `YEARLY`, `MONTHLY`, `WEEKLY`, `DAILY`, `HOURLY`, `MINUTELY`, `SECONDLY`.
- `dtstart`: Начальная дата и время для рассчитываемых событий.
- `interval`: Интервал между повторениями. Например, `interval=2` с `freq=MONTHLY` будет означать каждый второй месяц.
- `wkst`: День начала недели, который влияет на расчеты, основанные на недельной частоте.
- `count`: Количество повторений события.
- `until`: Конечная дата, после которой события не будут генерироваться.

 **Расширенные параметры**:
- `bysetpos`, `bymonth`, `bymonthday`, `byyearday`, `byeaster`, `byweekno`, `byweekday`, `byhour`, `byminute`, `bysecond`: Эти параметры позволяют детализировать правила повторения, указывая конкретные дни, месяцы, часы и так далее.

```python
from dateutil.rrule import rrule, MONTHLY
from datetime import datetime

# Создаем правило для повторения события каждый месяц
rule = rrule(freq=MONTHLY, count=6, dtstart=datetime(2024, 1, 1))

# Выводим все даты событий
for dt in rule:
    print(dt)
```
```
2024-01-01 00:00:00
2024-02-01 00:00:00
2024-03-01 00:00:00
2024-04-01 00:00:00
2024-05-01 00:00:00
2024-06-01 00:00:00
```
Этот код создаст серию из 6 событий, которые будут храниться в объектах `datetime`, которые в свою очередь будут храниться в итераторе

### rruleset()
Позволяет создавать комплексные правила добавления дат в итератор:
```python
from dateutil.rrule import rruleset, rrule, DAILY, WEEKLY, MO
from datetime import datetime

# Создаем набор правил
ruleset = rruleset()

# Добавляем ежедневное правило повторения
ruleset.rrule(rrule(freq=DAILY, count=10, dtstart=datetime(2024, 1, 1)))

# Добавляем еженедельное правило повторения по понедельникам
ruleset.rrule(rrule(freq=WEEKLY, count=5, dtstart=datetime(2024, 1, 1), byweekday=MO))

# Исключаем конкретную дату
ruleset.exdate(datetime(2024, 1, 5))

# Выводим все даты событий
for dt in ruleset:
    print(dt)
```
```
2024-01-01 00:00:00
2024-01-02 00:00:00
2024-01-03 00:00:00
2024-01-04 00:00:00
2024-01-06 00:00:00
2024-01-07 00:00:00
2024-01-08 00:00:00
2024-01-09 00:00:00
2024-01-10 00:00:00
2024-01-15 00:00:00
2024-01-22 00:00:00
2024-01-29 00:00:00
```
Типо, все что нагенерируют сразу два экземпляра `rrule()` или больше будет объединено в один итератор в хронологическом порядке исключая то, что нагенерирует экземпляр  `exrule()`

В `rruleset` можно докинуть конкретные даты через `rdate` или исключить конкретный даты через `exdate`