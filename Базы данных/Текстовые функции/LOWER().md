#база_данных #реляционные #SQL #СУБД #текстовые_функции #LOWER

Принимает в качестве аргумента строку, переводит все ее символы в нижний регистр, и возвращает строку, состоящую только из символов в нижнем регистре
```SQL
SELECT LOWER('beegeek'),
       LOWER('BeeGeek'),
       LOWER('BEEGEEK');
```
```
+------------------+------------------+------------------+
| LOWER('beegeek') | LOWER('BeeGeek') | LOWER('BEEGEEK') |
+------------------+------------------+------------------+
| beegeek          | beegeek          | beegeek          |
+------------------+------------------+------------------+
```

### Эта функция есть в `PostgreSQL`, работает точно так же