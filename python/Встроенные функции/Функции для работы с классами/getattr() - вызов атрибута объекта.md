#Python #OOP #ООП #getattr


Функция `getattr()` принимает три аргумента:
- `obj` — объект
- `name` — имя атрибута
- `default` — значение по умолчанию

Функция возвращает значение атрибута `name` объекта `obj`. Если объект `obj` не имеет атрибута `name`, возвращается значение по умолчанию `default`. Если значение по умолчанию не указано, возбуждается исключение `AttributeError`
```python
class Cat:
    pass


cat = Cat()

cat.name = 'Кемаль'

print(getattr(cat, 'name'))
print(getattr(cat, 'age', None))
```
```
Кемаль
None
```
Так же, как и через точечную нотацию, с помощью функции `getattr()` можно получать значения атрибутов класса через экземпляры этого класса
```python
class Cat:
    night_vision = True
    paws_count = 4


cat = Cat()

print(getattr(cat, 'night_vision'))
print(getattr(cat, 'paws_count'))
```
```
True
4
```


