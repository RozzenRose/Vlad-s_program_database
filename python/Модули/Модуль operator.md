#модуль #python #operator

Содержит элементарные математические методы, которые можно вызывать, как методы

Подключение модуля:
```python
import operator
from operator import*
```

|                       |               |                      |
| --------------------- | ------------- | -------------------- |
| **Операция**          | **Синтаксис** | **Функция**          |
| Addition              | `a + b`       | `add(a, b)`          |
| Containment Test      | `obj in seq`  | `contains(seq, obj)` |
| Division              | `a / b`       | `truediv(a, b)`      |
| Division              | `a // b`      | `floordiv(a, b)`     |
| Exponentiation        | `a ** b`      | `pow(a, b)`          |
| Modulo                | `a % b`       | `mod(a, b)`          |
| Multiplication        | `a * b`       | `mul(a, b)`          |
| Negation (Arithmetic) | `-a`          | `neg(a)`             |
| Subtraction           | `a - b`       | `sub(a, b)`          |
| Ordering              | `a < b`       | `lt(a, b)`           |
| Ordering              | `a <= b`      | `le(a, b)`           |
| Equality              | `a == b`      | `eq(a, b)`           |
| Difference            | `a != b`      | `ne(a, b)`           |
| Ordering              | `a >= b`      | `ge(a, b)`           |
| Ordering              | `a > b`       | `gt(a, b)`           |
Может пригодится при использовании модуля [[Модуль funсtools|functools]]