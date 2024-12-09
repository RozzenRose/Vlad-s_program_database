#база_данных #реляционные #SQL #СУБД #CASE 
#### Работает в `MySQL` и `PostgreSQL`
Функция `IF()` удобна для быстрых проверок тех или иных выражений на истинность. Но для построения сложных условных конструкций функция `IF()` подходит не очень хорошо, в силу того, что каждое следующее условие будет вызывать новый уровень вложенности
```SQL
CASE <значение>
    WHEN <первое сравниваемое значение> THEN <результат>
    WHEN <второе сравниваемое значение> THEN <результат>
    ...
    WHEN <n-oe сравниваемое значение> THEN <результат>
    ELSE <значение по умолчанию>
END
```

Пример:
```
+----+------------------------+---------------------+-------+----------+
| id | title                  | author              | price | quantity |
+----+------------------------+---------------------+-------+----------+
| 1  | Fight Club             | Chuck Palahniuk     | 9.99  | 12       |
| 2  | The Catcher in the Rye | J.D. Salinger       | 3.49  | 1        |
| 3  | The Green Mile         | Stephen King        | 15.99 | 4        |
| 4  | The Great Gatsby       | F. Scott Fitzgerald | 7.99  | 3        |
| 5  | The Lord of the Rings  | J.R.R. Tolkien      | 29.99 | 0        |
+----+------------------------+---------------------+-------+----------+
```
```SQL
SELECT title, author,
       CASE
           WHEN quantity = 0 THEN 'Out of stock'
           WHEN quantity = 1 THEN 'One in stock'
           WHEN quantity BETWEEN 2 AND 5 THEN 'Little in stock'
           ELSE 'A lot in stock'
       END AS availability
FROM Books;
```
```
+------------------------+---------------------+-----------------+
| title                  | author              | availability    |
+------------------------+---------------------+-----------------+
| Fight Club             | Chuck Palahniuk     | A lot in stock  |
| The Catcher in the Rye | J.D. Salinger       | One in stock    |
| The Green Mile         | Stephen King        | Little in stock |
| The Great Gatsby       | F. Scott Fitzgerald | Little in stock |
| The Lord of the Rings  | J.R.R. Tolkien      | Out of stock    |
+------------------------+---------------------+-----------------+
```
### Оператор `CASE` как функция
Оператор `CASE` можно засунуть в другую функцию:
```SQL
SELECT title, author,
       UPPER(CASE
                 WHEN price < 5 THEN 'Cheap'
                 WHEN price BETWEEN 5 AND 15 THEN 'Regular'
                 ELSE 'Expensive'
             END) AS rate
FROM Books;
```
```
+------------------------+---------------------+-----------+
| title                  | author              | rate      |
+------------------------+---------------------+-----------+
| Fight Club             | Chuck Palahniuk     | REGULAR   |
| The Catcher in the Rye | J.D. Salinger       | CHEAP     |
| The Green Mile         | Stephen King        | EXPENSIVE |
| The Great Gatsby       | F. Scott Fitzgerald | REGULAR   |
| The Lord of the Rings  | J.R.R. Tolkien      | EXPENSIVE |
+------------------------+---------------------+-----------+
```
Можно даже строить сложные условия для сортировки при помощи `ORDER BY`

В качестве примера напишем запрос, который извлекает название книги и имя автора, при этом в результирующем наборе сначала должны быть указаны книги, названия которых начинаются с артикля `The`, а затем все остальные.
```SQL
SELECT title, author
FROM Books
ORDER BY CASE
             WHEN title LIKE 'The %' THEN 1
             ELSE 2
         END;
```
```
+------------------------+---------------------+
| title                  | author              |
+------------------------+---------------------+
| The Catcher in the Rye | J.D. Salinger       |
| The Green Mile         | Stephen King        |
| The Great Gatsby       | F. Scott Fitzgerald |
| The Lord of the Rings  | J.R.R. Tolkien      |
| Fight Club             | Chuck Palahniuk     |
+------------------------+---------------------+
```