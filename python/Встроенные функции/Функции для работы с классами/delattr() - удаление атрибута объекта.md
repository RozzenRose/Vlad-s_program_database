#Python #OOP #ООП #date 


Функция `delattr()` принимает два аргумента:
- `obj` — объект
- `name` — имя атрибута

Функция удаляет атрибут `name` у объекта `obj`. Если объект не имеет атрибута `name`, возбуждается исключение `AttributeError`. \
```python
class Cat:
    pass


cat = Cat()

cat.name = 'Кемаль'
cat.age = 1

print(cat.__dict__)

delattr(cat, 'name')
delattr(cat, 'age')

print(cat.__dict__)
```
```
{'name': 'Кемаль', 'age': 1}
{}
