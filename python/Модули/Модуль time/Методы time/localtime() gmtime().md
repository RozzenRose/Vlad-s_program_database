#метод #python Модуль #time



### localtime()
Если закинуть в `localtime()` вернет объект типа `struct_time`, который будет содержать дату, соответствующую количеству секунд с начала эпохи.  В локальном формате
```python
import time

result = time.localtime(1630387918)
print('Результат:', result)
print('Год:', result.tm_year)
print('Месяц:', result.tm_mon)
print('День:', result.tm_mday)
print('Час:', result.tm_hour)
```
```
Результат: time.struct_time(tm_year=2021, tm_mon=8, tm_mday=31, tm_hour=8, tm_min=31, tm_sec=58, tm_wday=1, tm_yday=243, tm_isdst=0)
Год: 2021
Месяц: 8
День: 31
Час: 8
```

### gmtime()
Работает  вообще один в один как `localtime()`, но вернет объект `struct_time` с содержанием даты в UTC формате, а не в локальном