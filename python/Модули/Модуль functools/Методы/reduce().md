#functools #python #reduce #метод

Вернет итерируемый объект, последовательно применив к нему указанную функцию.

Требует подкинуть [[Модуль funсtools]]
```python
from functools import reduce
reduce(func, numbers, start)
```
	func - функция 
	numbers - итерируемый объект
	start - начальное значение формирующегося объекта
```python
from functools import reduce

def func(a, b):
	return a + b

numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
total = reduce(func, numbers, 0) # в качестве начального значения 0
print(total)
```
Первый аргумент - это значение с прошлого цыкала! 
```
55
```

Можно юзать любые мат функции с [[Модуль operator]]

Документация по функции `reduce()` доступна по [ссылке](https://docs-python.ru/standart-library/modul-functools-python/funktsija-reduce-modulja-functools/).