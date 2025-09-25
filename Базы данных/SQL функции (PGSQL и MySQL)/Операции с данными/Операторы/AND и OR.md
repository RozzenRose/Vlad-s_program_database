#база_данных #реляционные #SQL #СУБД #WHERE #AND #OR 

```
+----+----------+----------------------+------------+---------+--------------+
| id | place    | trackname            | artist     | streams | release_date |
+----+----------+----------------------+------------+---------+--------------+
| 1  | 4        | Crazy On You         | Heart      | 76338   | 1976-03-01   |
| 2  | 3        | My Lover             | The Sounds | 99488   | 2009-05-31   |
| 3  | 2        | Running up That Hill | Kate Bush  | 121495  | 1985-08-05   |
| 4  | 5        | Thrill               | The Sounds | 49345   | 2016-11-10   |
| 5  | 1        | Spent the Day in Bed | Morrissey  | 174994  | 2017-09-19   |
+----+----------+----------------------+------------+---------+--------------+
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
VALUES (4, 'Crazy On You', 'Heart', 76338, '1976-03-01'),
       (3, 'My Lover', 'The Sounds', 99488, '2009-05-31'),
       (2, 'Running up That Hill', 'Kate Bush', 121495, '1985-08-05'),
       (5, 'Thrill', 'The Sounds', 49345, '2016-11-10'),
       (1, 'Spent the Day in Bed', 'Morrissey', 174994, '2017-09-19');
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
VALUES (4, 'Crazy On You', 'Heart', 76338, '1976-03-01'),
       (3, 'My Lover', 'The Sounds', 99488, '2009-05-31'),
       (2, 'Running up That Hill', 'Kate Bush', 121495, '1985-08-05'),
       (5, 'Thrill', 'The Sounds', 49345, '2016-11-10'),
       (1, 'Spent the Day in Bed', 'Morrissey', 174994, '2017-09-19');
```
### Оператор `AND`
Чтобы фильтровать данные по нескольким полям, необходимо воспользоваться оператором `AND`. Он использует для извлечения только тех записей, которые удовлетворяют **всем** указанным условиям
```MySQL
SELECT trackname, artist, streams, release_date
FROM Songs
WHERE streams > 50000 AND release_date >= '2000-01-01';
```
```
+----------------------+------------+---------+--------------+
| trackname            | artist     | streams | release_date |
+----------------------+------------+---------+--------------+
| My Lover             | The Sounds | 99488   | 2009-05-31   |
| Spent the Day in Bed | Morrissey  | 174994  | 2017-09-19   |
+----------------------+------------+---------+--------------+
```
Этот запрос извлекает данные о тех песнях, количество прослушиваний которых превышает `50000` и которые были выпущены в `2000` году или позднее.
### Оператор `OR`
Извлекает только те данные, которые удовлетворяют хотя бы одному условию
```MySQL
SELECT trackname, artist
FROM Songs
WHERE artist = 'Heart' OR artist = 'Kate Bush';
```
```
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy On You         | Heart     |
| Running up That Hill | Kate Bush |
+----------------------+-----------+
```
### Оператор `IN` и `NOT IN`
Поверяет совпадает ли значение поля с одним из перечисленных значений
```MySQL
SELECT trackname, artist
FROM Songs
WHERE artist IN ('Heart', 'Kate Bush', 'Morrissey');
```
```
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy on You         | Heart     |
| Running up That Hill | Kate Bush |
| Spent the Day in Bed | Morrissey |
+----------------------+-----------+
```
Этот запрос вытаскивает песни исполнителей `Heart, Kate Bush` и `Morrissey`. После оператора `IN` указан список этих исполнителей через запятую, а весь список заключен в скобки

Можно предположить, что оператор `IN` выполняет ту же функцию, что и логический оператор `OR`, и это действительно так. Приведенный выше запрос равносилен следующему запросу:
```MySQL
SELECT trackname, artist
FROM Songs
WHERE artist = 'Heart' OR artist = 'Kate Bush' OR artist = 'Morrissey';
```
```
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy on You         | Heart     |
| Running up That Hill | Kate Bush |
| Spent the Day in Bed | Morrissey |
+----------------------+-----------+
```
Однако оператор `IN` имеет некоторое преимущество перед оператором `OR`. Например, при работе с большим количеством значений синтаксис логического оператора `IN` гораздо понятнее. Также при использовании оператора `IN` совместно с операторами `AND` и `OR` намного легче управлять порядком их обработки.

`NOT IN` работает точно так же, но наоборот
### Оператор `NOT`
Логический оператор `NOT` служит только одной цели - отрицать условие, следующее за ним. например, с помощью данного оператора мы можем извлечь данные о тех песнях, исполнителем которых не является группа `The Sound`
```MySQL
SELECT trackname, artist
FROM Songs
WHERE NOT artist = 'The Sounds';
```
```
+----------------------+-----------+
| trackname            | artist    |
+----------------------+-----------+
| Crazy on You         | Heart     |
| Running up That Hill | Kate Bush |
| Spent the Day in Bed | Morrissey |
+----------------------+-----------+
```
Логический оператор `NOT` отрицает следующее за ним условие, поэтому возвращаются не те записи, которые в поле `artist` содержат значение `The Sounds`, а все остальные
### Примечания
**Примечание 1.** Логические операторы `AND, OR, NOT, IN` и `NOT IN` имеют разный приоритет. В таблице ниже они представлены в порядке уменьшения их приоритета:

|               |
| ------------- |
| **Оператор**  |
| `IN , NOT IN` |
| `NOT`         |
| `AND`         |
| `OR`          |

Если условие содержит несколько логических операторов с одинаковым приоритетом, они выполняются слева направо.

Так же порядок может быть определен скобками