#calendar #python #метод #setfirstweekend


По умолчанию в модуле `calendar` неделя начинается с понедельника с индексом `1`,  а заканчивается воскресеньем с индексом `6`

`setfritstweekend()` позволяет это изменить, например можно первый день установить как по кайфу
```python
import calendar
calendar.setfirstweekday(calendar.SUNDAY)     # эквивалентно calendar.setfirstweekday(6)
```
Да и все, это все зачем он нужен