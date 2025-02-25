#база_данных #реляционные #SQL #СУБД #дата #время #DATE #TIME 

Функция `DATE()` используется для получения даты из даты и времени. Она принимает в качестве аргумента дату и время, извлекает из него дату и возвращает полученный результат.
```sql
SELECT DATE('2023-10-20 12:30:00'),
       DATE('2023-12-31 10:00:20');
```
```
+-----------------------------+-----------------------------+
| DATE('2023-10-20 12:30:00') | DATE('2023-12-31 10:00:20') |
+-----------------------------+-----------------------------+
| 2023-10-20                  | 2023-12-31                  |
+-----------------------------+-----------------------------+
```

Если аргументом функции `DATE()` является дата без времени, функция вернет ее в исходном виде.
```sql
SELECT DATE('2023-10-20');
```
```
+--------------------+
| DATE('2023-10-20') |
+--------------------+
| 2023-10-20         |
+--------------------+
```

Похожим образом себя ведет функция `TIME()` за тем исключением, что она извлекает временное значение, а не дату.
```sql
SELECT TIME('2023-10-20 12:30:00'),
       TIME('2023-12-31 10:00:20'),
       TIME('12:30:00');
```
```
+-----------------------------+-----------------------------+------------------+
| TIME('2023-10-20 12:30:00') | TIME('2023-12-31 10:00:20') | TIME('12:30:00') |
+-----------------------------+-----------------------------+------------------+
| 12:30:00                    | 10:00:20                    | 12:30:00         |
+-----------------------------+-----------------------------+------------------+
```

### Этих функций нет в `PosgreSQL`

В PostgreSQL функции `DATE()` и `TIME()` отсутствуют, как в MySQL, но есть аналогичные способы работы с датами и временем.

Преобразование данных в дату и время в PostgreSQL

1. **Преобразование строки в дату**:
```PostgreSQL
SELECT '2023-07-15'::DATE AS date_value;
```
Альтернативный вариант:
```PostgreSQL
SELECT CAST('2023-07-15' AS DATE) AS date_value;
```
    
2. **Преобразование строки во время**:
```PostgreSQL
SELECT 
    '2023-10-20 12:30:00'::TIME AS time1,
    '2023-12-31 10:00:20'::TIME AS time2,
    '12:30:00'::TIME AS time3;

```
Альтернативный вариант:
```PostgreSQL
SELECT CAST('14:30:00' AS TIME) AS time_value;
```
