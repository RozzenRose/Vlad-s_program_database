#встроенная_функция #python #множества #frozenset

Метод преобразует итерируемый объект в неизменяемое множество (тип `frozen`). Вызов без аргументов возвращает пустое неизменяемое множество.
```python
print(frozenset())
print(frozenset('beegeek'))
print(frozenset(set('aaaabbccccccde')))
```
```
frozenset()
frozenset({'g', 'e', 'k', 'b'})
frozenset({'a', 'c', 'e', 'd', 'b'})
```