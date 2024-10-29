#python #оператор #match #case 


Позволяет писать сложную условную логику красиво и лаконично, без использования многоэтажных структур через базовый условный оператор `if...then...else`

Например, нам нужно определить в каком квадранте координатной плоскости находится точка. При том, что нам известны осевые координаты этой точки
```python
def quadrant_checker(x, y):  
    match (x, y):  
        case (x, y) if x > 0 and y > 0:  
            quadrant = 3  
        case (x, y) if x > 0 and y < 0:  
            quadrant = 2  
        case (x, y) if x < 0 and y > 0:  
            quadrant = 4  
        case (x, y) if x < 0 and y < 0:  
            quadrant = 1  
        case _:  
	        quadrant = 'Точка на ходится на одной из осей или в точке начала координат'
	return quadrant

print(quadrant_checker(4, 5))
print(quadrant_checker(4, -2))
print(quadrant_checker(-3, 5))
print(quadrant_checker(-6, -3))
print(quadrant_checker(4, 0))
```
```
3
2
4
1
Точка на ходится на одной из осей или в точке начала координат
```
`case _` это случай по умолчанию, если никакие условия выше не будут выполнены, выполнится это

Аргументы, которые получает `match` являются позиционными, то есть в `case` они могут называться как угодно, `match` передаст их туда чисто по позиционному принципу
```python
def quadrant_checker(a, b):  
    match (a, b):  
        case (x, y) if x > 0 and y > 0:  
            quadrant = 3  
        case (x, y) if x > 0 and y < 0:  
            quadrant = 2  
        case (x, y) if x < 0 and y > 0:  
            quadrant = 4  
        case (x, y) if x < 0 and y < 0:  
            quadrant = 1  
        case _:  
	        quadrant = 'Точка на ходится на одной из осей или в точке начала координат'
	return quadrant

print(quadrant_checker(4, 5))
print(quadrant_checker(4, -2))
print(quadrant_checker(-3, 5))
print(quadrant_checker(-6, -3))
print(quadrant_checker(4, 0))
```
```
3
2
4
1
Точка на ходится на одной из осей или в точке начала координат
```
Да, так это тоже будет работать

Проверять аргументы на прямое равенство внутри блока `case` вообще можно без дополнительных операторов
```python
def check_value(x):
    match x:
        case 1:
            return "Один"
        case 2:
            return "Два"
        case _:
            return "Неизвестное значение"

print(check_value(1))  # Вывод: Один
print(check_value(3))  # Вывод: Неизвестное значение
```
