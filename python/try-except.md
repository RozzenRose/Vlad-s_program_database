#python 

Позволяет выполнить код, который потенциально может вызвать ошибку безопасно. В случае, если ошибка будет сформирована, она будет просто перехвачена в блоке `except`
```python
from datetime import date, time

try:
	day, mount, year = input('Введите дату в формате ДД.ММ.ГГГГ').split('.')
	my_date = date(int(year), int(mount), int(day))
	print(my_date)
except ValueError:
	print('Ошибка ввода')
```
Исключение `ValueError` в этом коде может возникнуть в двух случаях, если функция `int()` не сможет преобразовать строку в число или если числа, которые введет пользователь невозможно будет преобразовать в дату. Но в случае, если одно из этих двух событий случиться программа не будет остановлена.
Остановится выполнение кода внутри блока `try`. Блок `except` начнет выполнение кода внутри себя.