#база_данных #реляционные #SQL #СУБД #дата #время #LAST_DAY

Функция `LAST_DAY()` используется для замены дня на последний день месяца. Она принимает в качестве аргумента дату, заменяет в ней день на последний день месяца этой даты и возвращает полученный результат.
```MySQL
SELECT LAST_DAY('2023-02-14'),
       LAST_DAY('2023-03-14'),
       LAST_DAY('2023-04-14');
```
```
+------------------------+------------------------+------------------------+
| LAST_DAY('2023-02-14') | LAST_DAY('2023-03-14') | LAST_DAY('2023-04-14') |
+------------------------+------------------------+------------------------+
| 2023-02-28             | 2023-03-31             | 2023-04-30             |
+------------------------+------------------------+------------------------+
```

### Этой функции нет в `PostgreSQL`
Нахождение последнего дня текущего месяца:
```PostgreSQL
SELECT (DATE_TRUNC('month', CURRENT_DATE) + INTERVAL '1 month - 1 day')::DATE AS last_day_of_month;
```
 Нахождение последнего дня заданного месяца:
```PostgreSQL
SELECT (DATE_TRUNC('month', DATE '2024-11-22') + INTERVAL '1 month - 1 day')::DATE AS last_day_of_given_month;
```