#collections #Python #ChainMap #объект

### Атрибут `.maps`
Объект `ChainMap` хранит все объединяемые словари во внутреннем списке. Этот список доступен через атрибут `.maps` и может быть изменен. Порядок словарей в списке `.maps` соответствует порядку, в котором словари были указаны при создании объекта `ChainMap`
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

print(pets)
print(pets.maps)
print(type(pets.maps))
```
```
ChainMap({'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3})
[{'dogs': 15, 'cats': 8, 'pythons': 9}, {'dogs': 7, 'cats': 2, 'tigers': 3}]
<class 'list'>
```
Атрибут `maps` является обычным списком, поэтому он поддерживает все основные операции со списками. Мы можем добавлять в него новые словари, удалять уже добавленные, а также изменять их порядок.
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

pets.maps.reverse()
pets.maps[0]['lions'] = 10
del pets.maps[1]['cats']

print(pets)
print(pets.maps)
```
```
ChainMap({'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9})
[{'dogs': 7, 'cats': 2, 'tigers': 3, 'lions': 10}, {'dogs': 15, 'pythons': 9}]
```
Атрибут `maps` можно использовать для обработки абсолютно всех значений во всех словарях. С помощью этого атрибута мы можем обойти поведение по умолчанию, заключающееся в получении (изменении) первого значения из первого словаря.
```python
from collections import ChainMap

for_adoption = {'dogs': 15, 'cats': 8, 'pythons': 9}
vet_treatment = {'dogs': 7, 'cats': 2, 'tigers': 3}

pets = ChainMap(for_adoption, vet_treatment)

for animals in pets.maps:
    for key, value in animals.items():
        print(key, '->', value)
```
```
dogs -> 15
cats -> 8
pythons -> 9
dogs -> 7
cats -> 2
tigers -> 3
```

При помощи этого атрибута можно засовывать новые объекты в `ChainMap`
```python
from collections import ChainMap

fruits = ChainMap({'apple': 10, 'banana': 20},
                  {'lemon': 10, 'pineapple': 15},
                  {'kiwi': 15, 'lime': 5})

fruits.maps.append({'mango': 20, 'melon': 20})

del fruits.maps[0]
del fruits.maps[1]

print(fruits)
```
### Метод `new_child()`
Этот метод добавляет в объект `ChainMap` новый словарь с конца, но он не модифицирует старый `ChainMap`, а возвращает новый.
```python
from collections import ChainMap

# Создаем несколько словарей
dic1 = {'a': 1, 'b': 2}
dic2 = {'b': 3, 'c': 4}
dic3 = {'c': 5, 'd': 6}

# Создаем ChainMap из словарей
chain_map = ChainMap(dic1, dic2)

# Добавляем новый словарь в начало цепочки
new_chain_map = chain_map.new_child(dic3)

# Теперь new_chain_map содержит dic3, dic1 и dic2
```
Если применить метод `chain_map()` без аргументов, в начало объекта `ChainMap` будет добавлен пустой словарь.

### Атрибут `.parents`
Атрибут `.parents` возвращает новый объект `ChainMap`, содержащий все словари, кроме первого. Это полезно для пропуска первого словаря при поиске ключей, обратен методу `new_child()`
```python
from collections import ChainMap

dad = {'name': 'Timur', 'age': 29}
mom = {'name': 'Rosaly', 'age': 28}
son = {'name': 'Soslan', 'age': 0}

family = ChainMap(son, dad, mom)

print(family)
print(family.parents)
print(type(family.parents))
```
```
ChainMap({'name': 'Soslan', 'age': 0}, {'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
ChainMap({'name': 'Timur', 'age': 29}, {'name': 'Rosaly', 'age': 28})
<class 'collections.ChainMap'>
```