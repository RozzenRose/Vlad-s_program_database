#встроенная_функция #python #filter #функция_высшего_порядка

Уберет из итерируемого объекта все значения, которые не соответствуют условиям. Возвращают итератор. Написана на `C`, выполняется быстро, памяти жрет мало

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

Важно помнить, что итераторы, которые возвращают функции `map()` и `filter()` можно обойти только один раз. При вторичной итерации мы будем получать уже пустой итератор.
```python
squares = map(lambda x: x ** 2, range(1, 10))
evens = filter(lambda x: x % 2 == 0, [9, 3, 45, 67, 12, 90, 87, 12, 45, 67])

print('Первичная распаковка (итерация): ', *squares)
print('Вторичная распаковка (итерация): ', *squares)

print('Первичное преобразование в список (итерация): ', list(evens))
print('Вторичное преобразование в список (итерация): ', list(evens))
```
```
Первичная распаковка (итерация):  1 4 9 16 25 36 49 64 81
Вторичная распаковка (итерация): 
Первичное преобразование в список (итерация):  [12, 90, 12]
Вторичное преобразование в список (итерация):  []