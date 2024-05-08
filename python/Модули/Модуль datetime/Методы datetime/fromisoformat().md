Модуль #datetime #python #fromisoformat

Самостоятельное преобразование данных  из строки в объекты типов `date` или `time` работает довольно неудобно, требует довольно громоздкого кода.

`fromisoformat` позволяет преобразовать строку с датой или временем формата ***ISO*** в объект `date` или `time`. 
```python
from datetime import date, time

my_date = date.fromisoformat('2020-01-31')
my_time = time.fromisoformat('10:20:30')

print(my_date)
print(my_time)
print(type(my_date))
print(type(my_time))
```
```
2020-01-31
10:20:30
<class 'datetime.date'>
<class 'datetime.time'>
```