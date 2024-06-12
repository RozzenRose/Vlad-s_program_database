#встроенная_функция #python #метод #callable

Метод `callable()` принимает в качестве аргумента некоторый объект, вернет `True`, если он вызываемый и `False` если он невызываемый.
```python
print(callable(int))
print(callable(list))
print(callable(100))
print(callable([1, 2, 3]))
```
```
True
True
False
False
```