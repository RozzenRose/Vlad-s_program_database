#база_данных #реляционные #SQL #СУБД #тип_данных #CAST 

`CAST()` используется для явного преобразования одного типа данных в другой. Это особенно полезно при работе с разными типами данных, когда требуется явное преобразование. Вот как это работает:
```sql
CAST(expression AS target_data_type)
```
- **expression**: выражение или значение, которое вы хотите преобразовать.
- **target_data_type**: целевой тип данных, в который нужно преобразовать значение.

### Примеры использования
1. **Преобразование строки в целое число**:
```sql
SELECT CAST('123' AS INTEGER) AS int_value;
```
В этом примере строка `'123'` преобразуется в целое число `123`.
    
2. **Преобразование целого числа в строку**:
```sql
SELECT CAST(123 AS TEXT) AS text_value;
```
Здесь целое число `123` преобразуется в строку `'123'`.
    
3. **Преобразование строки в дату**:
```sql
SELECT CAST('2024-11-21' AS DATE) AS date_value;
```
В этом случае строка `'2024-11-21'` преобразуется в дату.
    
4. **Преобразование числа в число с плавающей запятой**:
```sql
SELECT CAST(123 AS DOUBLE PRECISION) AS double_value;
```
В этом примере целое число `123` преобразуется в число с двойной точностью.

В `PostgreSQL` есть еще специальный оператор `::`, который является синонимом функции `CAST()`
- Для преобразования типов данных также можно использовать оператор `::`, который является синонимом `CAST()`:
```PostgreSQL
SELECT '123'::INTEGER AS int_value;
```
    
- Некоторые преобразования типов могут быть выполнены автоматически, когда PostgreSQL может неявно преобразовать типы данных.