#calendar #Python #метод #monthrange 

Возвращает день недели первого дня месяца и количество дней в месяце в объекте типа `tuple` для указанного года и месяца
```python
import calendar

print(calendar.monthrange(2022, 1))     # январь 2022 года
print(calendar.monthrange(2021, 9))     # сентябрь 2021 года
```
```
(5, 31)
(2, 30)
```