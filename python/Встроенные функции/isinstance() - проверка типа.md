#встроенные_функции

Проверяет тип объекта или значение на принадлежность к конкретному типу. Возвращает булевскую переменную.

Пример:
```python
isinstance(3, int)
isinstance(3.5, float)
isinstance('VLAD', str)

true
true
true
```

Важно отметить что при проверке объекта с помощью этой функции она учитывает наследование, например

```python
class A:
	pass

Class B(A):
	pass

t = B()
print(isinstance(t, A)) # вернет True
```