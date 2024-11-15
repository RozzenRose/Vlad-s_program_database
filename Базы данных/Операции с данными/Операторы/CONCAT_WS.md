#база_данных #реляционные #SQL #СУБД #CONCAT_WS 

Работает примерно как [[CONCAT]], но `CONTACT_WS` принимает символ, который будет использован для объединения  строк
```
+----+-------+----------------------+------------+-------+----------+
| id | place | trackname            | artist     | price | quantity |
+----+-------+----------------------+------------+-------+----------+
| 1  | 4     | Crazy On You         | Heart      | 2.00  | 31454    |
| 2  | 3     | My Lover             | The Sounds | 3.00  | 4558     |
| 3  | 2     | Running up That Hill | Kate Bush  | 1.00  | 15874    |
| 4  | 5     | Thrill               | The Sounds | 5.00  | 548      |
| 5  | 1     | Spent the Day in Bed | Morrissey  | 5.00  | 564797   |
+----+-------+----------------------+------------+-------+----------+
```

```MySQL
SELECT CONCAT_WS(', ', id, artist, trackname) AS song
FROM Songs;
```
```
+------------------------------------+
| song                               |
+------------------------------------+
| 1, Heart, Crazy On You             |
| 2, The Sounds, My Lover            |
| 3, Kate Bush, Running up That Hill |
| 4, The Sounds, Thrill              |
| 5, Morrissey, Spent the Day in Bed |
+------------------------------------+
```
### Примечания
**Примечание 1.** Если одно из объединяемых значений, переданных в функцию `CONCAT_WS()`, равняется `NULL`, при объединении оно будет проигнорировано.
```sql
SELECT CONCAT_WS('-', 1, NULL, 3) AS result1,
       CONCAT_WS('-', NULL, 'two', 3) AS result2,
       CONCAT_WS('-', 'one', 2, NULL) AS result3;
```
```
+---------+---------+---------+
| result1 | result2 | result3 |
+---------+---------+---------+
| 1-3     | two-3   | one-2   |
+---------+---------+---------+
```
Если же значение `NULL` примет разделитель, функция `CONCAT_WS()` вернет значение `NULL`.
```sql
SELECT CONCAT_WS(NULL, 1, 2, 3) AS result;
```
```n
+--------+
| result |
+--------+
| NULL   |
+--------+
```