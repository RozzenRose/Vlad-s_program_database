#модуль #datetime #python #dateutil #relativedate #date


Этот модуль содержит более продвинутые методы, для работы с с типом данных `datetime` из модуля [[Модуль datetime|datetime]]

Перед использованием, его нужно накатить:
```cmd
pip install python-dateutil
```
Для Linux удобнее так:
```Terminal
sudo pacman -S python-dateutil
```

Модуль разбит на несколько подклассов


```python
# нужно импортировать модуль datetime
from datetime import datetime, date


from dateutil.easter import easter

from dateutil import rrule
```


## Подклассы:
### relativdelta (круче timedelta)
Содержит объект [[relativdelta()]]
```python
from dateutil.relativedelta import relativedelta, MO, TU, WE, TH, FR, SA, SU
```
### easter 
Содержит метод, который, возвращает объект `date`, содержащий дату Пасхи указанного года
```python
from dateutil.easter import easter
western_easter_2024 = easter(2024, method=3)
print(western_easter_2024)
```
```
2024-03-31
```
### rapser
Содержит метод [[parser]]
```python
from dateutil.parser import parse
```