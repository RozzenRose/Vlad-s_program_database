#OOP #ООП #Python #магический_метод #атрибуты #getattr 


Метод `__getattr__()` вызывается только в двух случаях:
- если в теле метода `__getattribute__()` было возбуждено исключение `AttributeError`
- если метод `__getattr__()` был явно вызван в теле метода `__getattribute__()`

Применять метод `__getattr__()` можно по-разному. Самый простой вариант - возврат значения по умолчанию при обращении к несуществующему атрибуту, вместо привычного возбуждения исключения `AttributeError`
```python
class Cat:
    def __init__(self, name):
        self.name = name
        
    def __getattr__(self, attr):
        print(f'Возвращаю значение по умолчанию')
        return None


cat = Cat('Никся')

print(cat.name)                                         # обращение к существующему атрибуту
print(cat.age)                                          # обращение к несуществующему атрибуту
print(cat.breed)                                        # обращение к несуществующему атрибуту
```
```
Никся
Возвращаю значение по умолчанию
None
Возвращаю значение по умолчанию
None
```
Также с помощью метода `__getattr__()` можно имитировать наличие различных атрибутов, которыми объект на самом деле не обладает:
```python
class Cat:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed                              # порода кошки
        
    def __getattr__(self, attr):
        if attr == 'info':
            return f'Имя: {self.name}\nПорода: {self.breed}'
        raise AttributeError


cat = Cat('Никся', 'Девон рекс')

print(cat.info)                                         # обращение к несуществующему атрибуту
```
```
Имя: Никся
Порода: Девон рекс
```