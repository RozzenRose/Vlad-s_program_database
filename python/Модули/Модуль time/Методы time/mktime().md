#метод #python модуль #time #mktime


Принимает в качестве аргумента объект типа `struct_time` или объект типа `tuple` содержащий 9 значений, который могут быть конвертированы `struct_time`. А возвращает количество секунд, прошедших с начала эпохи в местном времени
```python
import time

time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)
time_obj = time.mktime(time_tuple)
print('Локальное время в секундах:', time_obj)
```
```
Локальное время в секундах: 1630377118.0
```
Обратная функция по отношению к методу `localtime()`
```python
import time

seconds = 1630377118

time_obj = time.localtime(seconds)            # возвращает struct_time
print(time_obj)

time_seconds = time.mktime(time_obj)          # возвращает секунды из struct_time
print(time_seconds)
```
```no-highlight
time.struct_time(tm_year=2021, tm_mon=8, tm_mday=31, tm_hour=5, tm_min=31, tm_sec=58, tm_wday=1, tm_yday=243, tm_isdst=0)
1630377118.0
```