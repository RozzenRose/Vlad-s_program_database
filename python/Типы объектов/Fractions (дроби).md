#изменяемый_объект #объект #тип_данных #python 

Представляет числовые значения в виде натуральных дробей.
Требует импорта [[Модуль fractions|модуля fractions]] методы там же
```python
from fractions import Fraction

num1 = Fraction(3, 4) # 3 - числитель, 4 - знаменатель
num2 = Fraction('0.55')
num3 = Fraction('1/9')
num4 = Fraction(0.34)
print(num1, num2, num3, num4, sep='\n')
```
```
3/4
11/20
1/9
6124895493223875/18014398509481984
```
Автоматически упрощает дроби:
```
75/100 ====> 3/4
```

Можно обратиться конкретно к знаменателю или числителю при помощи атрибутов.
Вот для примера код, который находит наименьший общий знаменатель:
```python
from fractions import Fraction
from math import lcm

# Создаем две дроби
frac1 = Fraction(1, 6)
frac2 = Fraction(1, 4)

# Находим НОЗ знаменателей
lcm_denominator = lcm(frac1.denominator, frac2.denominator)

print(f"Наименьший общий знаменатель: {lcm_denominator}")
```