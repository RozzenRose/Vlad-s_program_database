#модуль #calendar #Python #day_name #day_addr #month_name #month_addt #атрибуты_дней_недели

Ну конечно, этот модуль содержит полезные типы данных и методы для работы с календарем, а ты че думал?

В отличии от модулей `datetime` и `time` которые так же содержат инструменты для работы с датами, модуль `calendar` содержит инструменты для отображения и манипулирования календарями.

Импорт:
```python
import calendar 
```
Этот модуль содержит функциональные атрибуты

Документация:
[calendar — General calendar-related functions — Python 3.8.19 documentation](https://docs.python.org/3.8/library/calendar.html)

[[Методы calendar]]
### Атрибуты
это как методы, но лучше)
#### calendar.day_name дни недели
Атрибут `calendar.day_name` возвращает `итератор`, содержащий названия дней недели на английском языке.
```python
import calendar

for name in calendar.day_name:
    print(name)
```
```
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday
```
но через [[Модуль locale|локализацию]] можно вытащить дни недели на русском
```python
import calendar, locale

locale.setlocale(locale.LC_ALL, 'ru_RU.UTF-8')

for name in calendar.day_name:
    print(name)
```
```
понедельник
вторник
среда
четверг
пятница
суббота
воскресенье
```
Заметил, что дни недели на инглише записаны с большой буквы, а на русском с маленькой, это русофобия кстати! Суки! Фиксится строковым методом `title()`
#### calendar.day_addr короткие дни недели
Вернет итератор, содержащий сокращенные названия дней недели.
```python
import calendar, locale

for name in calendar.day_abbr:
    print(name)

locale.setlocale(locale.LC_ALL, 'ru_RU.UTF-8')

for name in calendar.day_abbr:
    print(name)
```
```
Mon
Tue
Wed
Thu
Fri
Sat
Sun
Пн
Вт
Ср
Чт
Пт
Сб
Вс
```
Вот тут он возвращает дни недели с большой буквы на русском, молодцы!
#### calendar.month_name сокращенные месяцы
Вернет итератор, который будет содержать названия месяцев. `month_adde` сокращенные названия месяцев
```python
import calendar, locale

english_names = list(calendar.month_abbr)
print(english_names)

locale.setlocale(locale.LC_ALL, 'ru_RU.UTF-8')

russian_names = list(calendar.month_abbr)
print(russian_names)
```
```
['', 'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
['', 'янв', 'фев', 'мар', 'апр', 'май', 'июн', 'июл', 'авг', 'сен', 'окт', 'ноя', 'дек']
```

#### Атрибуты номеров дней недели
возвращают номера дней недели
```python
import calendar

print(calendar.MONDAY)
print(calendar.TUESDAY)
print(calendar.WEDNESDAY)
print(calendar.THURSDAY)
print(calendar.FRIDAY)
print(calendar.SATURDAY)
print(calendar.SUNDAY)
```
```
0
1
2
3
4
5
6
```

#### Атрибуты поддерживают индексацию
```python
import calendar

print(calendar.day_name[1])
print(calendar.day_abbr[1])
print(calendar.month_name[1])
print(calendar.month_abbr[1])
```
```

Tuesday
Tue
January
Jan
```