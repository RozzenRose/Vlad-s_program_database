#метод #python модуль #time #asctime


Принимает объект `struct_time` или `tuple`, содержащий 9 объекетов, которые могут быть конвертированы в `struct_time` и возвращает строку, содержащую дату и время:
```python
import time

time_tuple = (2021, 8, 31, 5, 31, 58, 1, 243, 0)

result = time.asctime(time_tuple)
print('Результат:', result)
```
```no-highlight
Результат: Tue Aug 31 05:31:58 2021
```
Работает один в один, как метод `ctime()`, но `ctime()` принимает на вход количество секунд, а `asctime()` объект типа `struct_time`