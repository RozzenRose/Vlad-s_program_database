#встроенная_функция #python #метод #min #функция_высшего_порядка

Находит наименьшее значение в итерируемом объекте. 
```python
max(data, key=, default=)
```
- `data`- итерируемый объект
- `key` - ссылка на функцию сравнивания
- `default` - значение, которое будет возвращено, в случае, если минимальное значение найдено не будет.

```python
print(min([1, 2, 5, 7, 34, 6]))
print(min('a', 'b', 'ab'))
print(min([-3, 4, -90, 3, 45], key=abs))
print(min([], default='Empty'))
```
```
1
a
-3
Empty
```
Если аргумент `default` не указан, то при передаче в функцию `min()` пустого итерируемого объекта произойдет ошибка.