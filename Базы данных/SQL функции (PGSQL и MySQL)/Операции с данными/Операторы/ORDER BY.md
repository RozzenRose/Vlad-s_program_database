#база_данных #реляционные #SQL #СУБД #ORDER_BY #ACS #DESC

В примерах будет использована эта таблица:
```
+----+-------+----------------------+------------+---------+--------------+
| id | place | trackname            | artist     | streams | release_date |
+----+-------+----------------------+------------+---------+--------------+
| 1  | 4     | Crazy On You         | Heart      | 76338   | 2009-12-19   |
| 2  | 3     | My Lover             | The Sounds | 99488   | 2009-05-31   |
| 3  | 2     | Running up That Hill | Kate Bush  | 121495  | 1985-08-05   |
| 4  | 5     | Thrill               | The Sounds | 49345   | 2016-11-11   |
| 5  | 1     | Spent the Day in Bed | Morrissey  | 174994  | 2017-10-17   |
+----+-------+----------------------+------------+---------+--------------+
```
##### Скрипт создания таблицы для примера:
```MySQL
DROP TABLE IF EXISTS Songs;
CREATE TABLE Songs
(
    id           INT PRIMARY KEY AUTO_INCREMENT,
    place        INT,
    trackname    VARCHAR(30),
    artist       VARCHAR(30),
    streams      INT,
    release_date DATE
);

INSERT INTO Songs (place, trackname, artist, streams, release_date)
VALUES (4, 'Crazy On You', 'Heart', 76338, '2009-12-19'),
       (3, 'My Lover', 'The Sounds', 99488, '2009-05-31'),
       (2, 'Running up That Hill', 'Kate Bush', 121495, '1985-08-05'),
       (5, 'Thrill', 'The Sounds', 49345, '2016-11-11'),
       (1, 'Spent the Day in Bed', 'Morrissey', 174994, '2017-10-17');
```
```PostgreSQL
DROP TABLE IF EXISTS "Songs";
CREATE TABLE "Songs"
(
    id           SERIAL PRIMARY KEY,
    place        INT,
    trackname    VARCHAR(30),
    artist       VARCHAR(30),
    streams      INT,
    release_date DATE
);

INSERT INTO "Songs" (place, trackname, artist, streams, release_date)
VALUES (4, 'Crazy On You', 'Heart', 76338, '2009-12-19'),
       (3, 'My Lover', 'The Sounds', 99488, '2009-05-31'),
       (2, 'Running up That Hill', 'Kate Bush', 121495, '1985-08-05'),
       (5, 'Thrill', 'The Sounds', 49345, '2016-11-11'),
       (1, 'Spent the Day in Bed', 'Morrissey', 174994, '2017-10-17');
```
### Оператор ORDER BY
Позволяет извлекать данные из **БД** в отсортированном виде, используется вместе с оператором [[SELECT]]
```sql
SELECT artist, trackname, release_date
FROM Songs
ORDER BY release_date;
```
```
+------------+----------------------+--------------+
| artist     | trackname            | release_date |
+------------+----------------------+--------------+
| Kate Bush  | Running up That Hill | 1985-08-05   |
| The Sounds | My Lover             | 2009-05-31   |
| Heart      | Crazy On You         | 2009-12-19   |
| The Sounds | Thrill               | 2016-11-11   |
| Morrissey  | Spent the Day in Bed | 2017-10-17   |
+------------+----------------------+--------------+
```
Этот код возвращает ответ в отсортированном виде по дате рождения. `ORDER BY` по-дефолту сортирует ответ по возрастанию

В запросе оператор `ORDER BY` должен следовать после операторов `SELECT` и `FROM`, в противном случае запрос завершится с ошибкой

Поля, по которым выполняется сортировка, необязательно должны быть извлечены, то есть сортировка может выполняться и по тем полям, которые не попадают в результирующую таблицу
```sql
SELECT artist, trackname
FROM Songs
ORDER BY release_date;
```
```
+------------+----------------------+
| artist     | trackname            |
+------------+----------------------+
| Kate Bush  | Running up That Hill |
| The Sounds | My Lover             |
| Heart      | Crazy On You         |
| The Sounds | Thrill               |
| Morrissey  | Spent the Day in Bed |
+------------+----------------------+
```
Здесь сортировка выполняется по полю `release_date`, однако извлечение этого поля не выполняется
### Сортировка по нескольким полям
Для выполнения сортировки по нескольким полям, нужно перечислить имена этих полей через запятую
```sql
SELECT artist, trackname, streams
FROM Songs
ORDER BY artist, streams;
```
```
+------------+----------------------+---------+
| artist     | trackname            | streams |
+------------+----------------------+---------+
| Heart      | Crazy On You         | 76338   |
| Kate Bush  | Running up That Hill | 121495  |
| Morrissey  | Spent the Day in Bed | 174994  |
| The Sounds | Thrill               | 49345   |
| The Sounds | My Lover             | 99488   |
+------------+----------------------+---------+
```
### Сортировка по положению поля
Вместо имен полей оператор `ORDER BY` позволяет указывать порядковые номера полей, по которым необходимо выполнить сортировку. Однако сортировка таким методом только по тем полям, которые присутствуют в результирующей таблице. Нумеруются поля с единицы. 
```sql
SELECT artist, trackname, streams
FROM Songs
ORDER BY 1, 3;
```
```
+------------+----------------------+---------+
| artist     | trackname            | streams |
+------------+----------------------+---------+
| Heart      | Crazy On You         | 76338   |
| Kate Bush  | Running up That Hill | 121495  |
| Morrissey  | Spent the Day in Bed | 174994  |
| The Sounds | Thrill               | 49345   |
| The Sounds | My Lover             | 99488   |
+------------+----------------------+---------+
```
### Указания направления сортировки
По-дефолту сортировка выполняется по возрастанию. Что бы сортировка выполнялась в порядке убывания, необходимо после имени поля казать ключевое слов `DESC`
```sql
SELECT place, trackname
FROM Songs
ORDER BY place DESC;
```
```
+-------+----------------------+
| place | trackname            |
+-------+----------------------+
| 5     | Thrill               |
| 4     | Crazy On You         |
| 3     | My Lover             |
| 2     | Running up That Hill |
| 1     | Spent the Day in Bed |
+-------+----------------------+
```
В примере записи сортируются по убыванию значения поля `place`

Важной особенностью ключевого слова `DESC` является то, что оно применяется только к тому полю, после которого стоит. Это необходимо учитывать, когда сортировка выполняется по нескольким полям
```sql
SELECT id, artist, trackname
FROM Songs
ORDER BY artist, id DESC;
```
```n
+----+------------+----------------------+
| id | artist     | trackname            |
+----+------------+----------------------+
| 1  | Heart      | Crazy On You         |
| 3  | Kate Bush  | Running up That Hill |
| 5  | Morrissey  | Spent the Day in Bed |
| 4  | The Sounds | Thrill               |
| 2  | The Sounds | My Lover             |
+----+------------+----------------------+
```
Здесь сортировка выполняется по полям `artist` и `id`. Ключевое слово `DESC` указано лишь после поля `id`, поэтому сортировка происходит в порядке **возрастания** значения поля `artist`, а в случае совпадения — в порядке **убывания** значения поля `id`.

Если необходимо отсортировать записи в порядке убывания значений нескольких полей, нужно обязательно указать ключевое слово `DESC` после каждого из них.

### Примечания

**Примечание 1.** Аналогично тому, как для сортировки в порядке убывания существует ключевое слово `DESC`, для сортировки в порядке возрастания предусмотрено ключевое слово `ASC`, использовать которое необязательно. 
```sql
SELECT trackname
FROM Songs
ORDER BY trackname;
```   
```sql
SELECT trackname
FROM Songs
ORDER BY trackname ASC;
```

**Примечание 2.** Операторы `ORDER BY` и `LIMIT` могут использоваться вместе для решения различных задач. Например, с их помощью можно определить две песни, которые вышли позже всех остальных. Для этого сперва необходимо отсортировать музыкальные композиции в порядке убывания даты их выхода, а затем ограничить полученный результат до двух значений.
```sql
SELECT artist, trackname, release_date
FROM Songs
ORDER BY release_date DESC
LIMIT 2;
```
```
+------------+----------------------+--------------+
| artist     | trackname            | release_date |
+------------+----------------------+--------------+
| Morrissey  | Spent the Day in Bed | 2017-10-17   |
| The Sounds | Thrill               | 2016-11-11   |
+------------+----------------------+--------------+
```
Стоит отметить, что подобные манипуляции с данными возможны благодаря порядку, в котором запрос выполняет свои операции: сначала запрос извлекает записи из таблицы, затем сортирует и только после этого ограничивает определенным количеством.

**Примечание 3.** Сортировка является одной из завершающих операций и выполняется уже после формирования результирующей таблицы, поэтому в блоке оператора `ORDER BY` можно указывать как фактические имена полей таблицы, из которой извлекаются данные, так и их псевдонимы.
```sql
SELECT artist, trackname AS track, release_date
FROM Songs
ORDER BY track;
```
```
+------------+----------------------+--------------+
| artist     | track                | release_date |
+------------+----------------------+--------------+
| Heart      | Crazy On You         | 2009-12-19   |
| The Sounds | My Lover             | 2009-05-31   |
| Kate Bush  | Running up That Hill | 1985-08-05   |
| Morrissey  | Spent the Day in Bed | 2017-10-17   |
| The Sounds | Thrill               | 2016-11-11   |
+------------+----------------------+--------------+
```
Если псевдоним состоит из нескольких слов, то при сортировке данных по этому псевдониму необходимо заключать его в обратные апострофы (` `` `).
```sql
SELECT artist, trackname AS 'track name'
FROM Songs
ORDER BY `track name`;
```
```
+------------+----------------------+
| artist     | track name           |
+------------+----------------------+
| Heart      | Crazy On You         |
| The Sounds | My Lover             |
| Kate Bush  | Running up That Hill |
| Morrissey  | Spent the Day in Bed |
| The Sounds | Thrill               |
+------------+----------------------+
```

**Примечание 4.** Ключевое слово `DESC` — это сокращение от `DESCENDING` (убывание). Его антоним – ключевое слово `ASC` – сокращение от `ASCENDING` (возрастание).

**Примечание 5.** При сортировке строковых значений регистр не учитывается. Данное поведение может быть изменено в настройках СУБД, однако выбирать, учитывать регистр или нет, с помощью оператора `ORDER BY` нельзя.