#база_данных #реляционные #SQL #СУБД #текстовые_функции #RIGHT

Функция `RIGHT()` используется для извлечения определенного количества символов из конца строки. Она принимает два аргумента в следующем порядке:
- `str`- исходная строка
- `count` - количество извлекаемых символов

Функция возвращает строку, состоящую из последних `count` символов строки `str`.
```sql
SELECT RIGHT('beegeek', 1),
       RIGHT('beegeek', 3),
       RIGHT('beegeek', 7);
```
```
+---------------------+---------------------+---------------------+
| RIGHT('beegeek', 1) | RIGHT('beegeek', 3) | RIGHT('beegeek', 7) |
+---------------------+---------------------+---------------------+
| k                   | eek                 | beegeek             |
+---------------------+---------------------+---------------------+
```
Если количество извлекаемых символов меньше `1`, функция `RIGHT()` вернет пустую строку
Если количество извлекаемых символов больше длины строки, функция `RIGHT()` вернет всю строку
