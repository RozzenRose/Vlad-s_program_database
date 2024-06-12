#python #оператор

Оператор `is` сравнивает `id` двух объектов
Для того что бы узнать `id` объекта достаточно применить [[id() - id объекта|функцию id()]]
```python
>>> x = "Holberton"
>>> y = "Holberton"
>>> id(x)
140135852055856
>>> id(y)
140135852055856
>>> print(x is y) '''comparing the types'''
True
```