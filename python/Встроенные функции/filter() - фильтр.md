#встроенная_функции #python

Уберет из итерируемого объекта все значения, которые не соответствуют условиям.

```python
filter(func, numbers)
```
	func - функция содержащая условие для фильтрации
	numbers - итерируемый объект
вернет итератор!

Пример:
```python
def func(elem):
	return elem >= 0

numbers = [-1, 2, -3, 4, 0, -20, 10]
positive_numbers = list(filter(func, numbers)) # преобразуем итератор в список
print(positive_numbers)
```

```
[2, 4, 0, 10]
```

Вообще вывод такой же как в функции [[map() - модификатор|map()]]