#встроенная_функция #python #zip #метод


Объединяет объекты из каждого переданного методу итерируемого объекта в кортежи. 
Вернет итератор кортежей, прогони через цикл или передай в итерируемый объект.
![[Pasted image 20240419131632.png]]
```python
zip(iterable1, iterable2)
```
Пример:
```python
numbers = [1, 2, 3]
words = ['one', 'two', 'three']

for pair in zip(numbers, words):
	print(pair)

print(list(zip(number, word)))
```
```
(1, 'one')
(2, 'two')
(3, 'three')
[(1, 'one'), (2, 'two'), (3, 'three')]
```
Можно передать больше двух итерируемых объектов