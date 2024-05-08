#calendar #python #метод #firstweekday


Возвращает номер дня недели, который сейчас установлен в качестве дня начала недели
```python
import calendar

print(calendar.firstweekday())
calendar.setfirstweekday(calendar.SUNDAY)
print(calendar.firstweekday())
```
```
0
6
```