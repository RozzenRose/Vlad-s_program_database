#модуль #python #fractions #дроби


Возвращает самую близкую к указанному числу дробь, чей знаменатель не будет превосходить число, переданное методу в качестве аргумента.

```python
from fractions import Fraction
import math

num = Fraction(str(math.pi))
limited = num.limit_dominator(50)
print(limited)
```
```
22/7
```
