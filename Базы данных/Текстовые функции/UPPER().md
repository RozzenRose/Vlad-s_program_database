#база_данных #реляционные #SQL #СУБД #текстовые_функции #UPPER 

Принимает в качестве аргумента строку, переводит все ее символы в верхний регистр, и возвращает строку, состоящую только из символов в верхнем регистре
```sql
SELECT UPPER('beegeek'),
       UPPER('BeeGeek'),
       UPPER('BEEGEEK');
```
```
+------------------+------------------+------------------+
| UPPER('beegeek') | UPPER('BeeGeek') | UPPER('BEEGEEK') |
+------------------+------------------+------------------+
| BEEGEEK          | BEEGEEK          | BEEGEEK          |
+------------------+------------------+------------------+
```