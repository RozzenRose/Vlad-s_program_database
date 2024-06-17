#модуль #python 

![[Pasted image 20240614012303.png|500]]
Содержит кучу математических методов.

Подключение модуля:
```python
import math
from math import*
```

Все методы модуля:
```python
math.ceil(x)      # Округление вверх
math.floor(x)     # Округление вниз
math.comb(n, k)   # Количество сочетаний из n по k
math.copysign(x, y) # Возвращает число с модулем x и знаком y
math.fabs(x)      # Абсолютное значение
math.factorial(n) # Факториал числа n
math.fmod(x, y)   # Остаток от деления x на y
math.frexp(x)     # Мантисса и экспонента числа
math.fsum(iterable) # Точная сумма значений в итерируемом
math.isfinite(x)  # Проверка на конечность
math.isinf(x)     # Проверка на бесконечность
math.isnan(x)     # Проверка на NaN (не число)
math.modf(x)      # Дробная и целая часть числа
math.remainder(x, y) # Разность x и ближайшего кратного y
math.exp(x)       # \(e^x\), где e - основание натурального логарифма
math.expm1(x)     # \(e^x - 1\)
math.log(x[, base]) # Логарифм x по основанию base
math.log1p(x)     # Натуральный логарифм \(1 + x\)
math.log2(x)      # Логарифм x по основанию 2
math.log10(x)     # Логарифм x по основанию 10
math.pow(x, y)    # \(x^y\)
math.sqrt(x)      # Квадратный корень из x
math.acos(x)      # Арккосинус x
math.asin(x)      # Арксинус x
math.atan(x)      # Арктангенс x
math.atan2(y, x)  # Арктангенс отношения y/x
math.cos(x)       # Косинус x
math.hypot(x, y)  # Евклидово расстояние от начала координат до (x, y)
math.sin(x)       # Синус x
math.tan(x)       # Тангенс x
math.degrees(x)   # Преобразование радиан в градусы
math.radians(x)   # Преобразование градусов в радианы
math.acosh(x)     # Обратный гиперболический косинус x
math.asinh(x)     # Обратный гиперболический синус x
math.atanh(x)     # Обратный гиперболический тангенс x
math.cosh(x)      # Гиперболический косинус x
math.sinh(x)      # Гиперболический синус x
math.tanh(x)      # Гиперболический тангенс x
math.erf(x)       # Функция ошибок
math.erfc(x)      # Дополнительная функция ошибок
math.gamma(x)     # Гамма-функция x
math.lgamma(x)    # Натуральный логарифм гамма-функции x
math.pi           # Константа π
math.e            # Константа e
math.tau          # Константа τ (2π)
math.inf          # Положительная бесконечность
math.lmc(a, b)    # Находит наимешьшее число, кратное обоим переданным аргументам
```
