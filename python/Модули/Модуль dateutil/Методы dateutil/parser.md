#метод #datetime #python #dateutil #date #parser


Метод содержится к одноименном подклассе модуля `dateutil`. Предназначен для сборки объектов типа `datetime` на основе строк, содержащих данные о дате и времени
```python
from dateutil.parser import parse

# Разбор строки с датой и временем
dt = parse("2024-05-08 17:15:01")
print(dt)  # Выведет объект datetime для указанной даты и времени
```

### Параметры:

#### default
`default`: Этот параметр позволяет задать объект `datetime`, который будет использоваться в качестве основы для элементов, не указанных в строке. [Если в строке не указаны некоторые компоненты даты или времени, они будут взяты из этого объекта `default`](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html)
```python
from dateutil.parser import parse
from datetime import datetime

default_time = datetime(2024, 5, 8, 17, 15, 1)
date_string = "2024-05"
dt = parse(date_string, default=default_time)

print(dt)  # Выведет 2024-05-08 17:15:01
```
#### ignoretz
 `ignoretz`: [Если установлено значение `True`, информация о часовом поясе в разбираемой строке будет игнорироваться, и будет возвращен объект `datetime` без информации о часовом поясе](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html).
#### tzinfos
`tzinfos`: Этот параметр позволяет задать дополнительные названия или псевдонимы часовых поясов, которые могут присутствовать в строке. [Это может быть словарь, сопоставляющий названия часовых поясов с объектами `tzinfo`, или функция, которая принимает название часового пояса и смещение и возвращает объект `tzinfo`](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html).
```python
from dateutil.parser import parse
from dateutil.tz import gettz

# Дополнительные часовые пояса
tzinfos = {"BRST": -7200, "CST": gettz("America/Chicago")}

# Разбор строки с указанием часового пояса
dt = parse("2024-05-08 17:15:01 BRST", tzinfos=tzinfos)
print(dt)  # Выведет объект datetime с учетом часового пояса BRST
```
```
2024-05-08 17:15:01-02:00
```
#### dayfirst и yearfirst
`dayfirst` и `yearfirst`: Эти параметры указывают, следует ли интерпретировать первое число в неоднозначной дате как день (`dayfirst=True`) или год (`yearfirst=True`). [Это особенно полезно для разбора строк с датами, где используются разные порядки даты, например, `dd/mm/yyyy` против `mm/dd/yyyy`](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html).
#### fuzzy
`fuzzy`: [Если установлено значение `True`, разрешается нечеткий разбор, что позволяет игнорировать некоторые несущественные элементы строки](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html).
Полезно, когда строка содержит лишний текст помимо инфы о дате и времени
```python
from dateutil.parser import parse

date_string = "Today is January 1, 2047 at 8:21:00AM"
dt = parse(date_string, fuzzy=True)

print(dt)  # Выведет 2047-01-01 08:21:00
```
#### fuzzy_with_token
`fuzzy_with_tokens`: [Если установлено значение `True`, включается нечеткий разбор, и парсер возвращает кортеж, где первый элемент — это разобранная отметка даты/времени, а второй элемент — кортеж, содержащий игнорируемые части строки](https://dateutil.readthedocs.io/en/stable/parser.html)[1](https://dateutil.readthedocs.io/en/stable/parser.html).