#встроенная_функция #python #метод #hasattr

Метод `hasatar()` используется для проверки существования атрибута. Она принимает два аргумента:
- `object` — объект, в котором нужно проверить существование атрибута
- `name` — имя проверяемого атрибута

Метод возвращает `True`, если `object` имеет атрибут `name` или `False` в противном случае.
```python
print(hasattr('stepik', 'isalpha'))
print(hasattr([1, 2, 3], 'sort'))
print(hasattr(13, 'to_str'))
```
```
True
True
False
```