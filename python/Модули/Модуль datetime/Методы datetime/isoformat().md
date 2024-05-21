Модуль #datetime #python #метод #isoformat


Преобразует объекты типа `datetime` в строки  датой и временем в формате ISO
```python
import datetime

# Создание объекта datetime
dt = datetime.datetime(2024, 5, 20, 11, 42, 48)

# Преобразование объекта datetime в строку ISO 8601
iso_string = dt.isoformat()

print(iso_string)
```
```
2024-05-20T11:42:48
```