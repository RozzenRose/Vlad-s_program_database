#python #оператор #if #then #else

Оператор `if...then...else` используется для выполнения разных блоков кода в зависимости от выполнения или невыполнения условия. Так же его иногда называют `услоыный оператор`
```python
x = 10
if x > 5:
    print("x больше 5")
else:
    print("x меньше или равно 5")
```
```
x больше 5
```
Если условие, указанное в блоке `if` выполняется, начинается выполнение кода в блоке `if`, в случае если условие не выполняется, начинается выполнение кода в блоке `else`

Можно построить чуть более сложную логическую конструкцию
```python
x = 10
if x > 15:
    print("x больше 15")
elif x > 5:
    print("x больше 5, но меньше или равно 15")
else:
    print("x меньше или равно 5"
```
```
x больше 5, но меньше или равно 15
```
В этом случае, интерпретатор проверяет 