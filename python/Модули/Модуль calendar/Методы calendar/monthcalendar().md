#calendar #Python #метод #monthrcalendar


Вернет матрицу, представляющую календарь на месяц, который был указан. Каждая строка - это неделя. Матрица выглядит, как объект `list` который набит другими объектами `list`

```python
import calendar

print(*calendar.monthcalendar(2021, 9), sep='\n')
```
```
[0, 0, 1, 2, 3, 4, 5]
[6, 7, 8, 9, 10, 11, 12]
[13, 14, 15, 16, 17, 18, 19]
[20, 21, 22, 23, 24, 25, 26]
[27, 28, 29, 30, 0, 0, 0]
```