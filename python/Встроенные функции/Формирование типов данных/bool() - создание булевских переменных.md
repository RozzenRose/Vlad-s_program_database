#встроенная_функция #python #булевские #дискреты #bool

Метод возвращает логическое значение переданного объекта. Объект будет возвращать `False`, если :
- объект пуст - `{}`, `[]`, `()`
- объект - `False`
- объект равен `0`
- объект - `None`
```python
print(bool('Beegeek'))
print(bool(17))
print(bool(['apple', 'cherry']))
print(bool())
print(bool(''))
print(bool(0))
print(bool([]))
```
```
True
True
True
False
False
False
False
```