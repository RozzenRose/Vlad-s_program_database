#встроенная_функция #python #метод #split 


Метод `split()` делит строку на подстроки, по указанному символу, и возвращает список подстрок в объекте типа `list()`

Аргументы:
- `symbol` - указываем символ или набор символ, по которому будет происходить разделение строки на подстроки, естественно это объект типа `str`

Если `symbol` не указан, разделение по-дефолту произойдет по символу пробела
```python
print('stepik beegeek       python'.split())             # по умолчанию разбиваем через пробельные символы
print('aaa,bbbb,ccccc'.split(','))
print('hello---world---from---beegeek'.split('---'))
```
```
['stepik', 'beegeek', 'python']
['aaa', 'bbbb', 'ccccc']
['hello', 'world', 'from', 'beegeek']
```

Мы можем использовать необязательный аргумент `maxsplit` для задания максимального количества разбиений строки
```python
text = 'foo;bar;baz;beegeek;stepik;python'

print(text.split(sep=';', maxsplit=2))
```
```
['foo', 'bar', 'baz;beegeek;stepik;python']
```