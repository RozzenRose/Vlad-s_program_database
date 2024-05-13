#python #файлы #работа_с_файлами #python #CSV #TSV #writer #специальный_обект


Специальный объект модуля [[Модуль CSV|CSV]], предназначенный для записи данных в CSV файл.

```python
import csv
rows = csv.reader(file, delimiter=';', quotechar='"', quoting=)
```
- `file` - название файла
- `delimiter` - межэлементный разделитель, по умолчанию `','`
- `quotechar` - односимвольная строка, используемая для кавычек в полях, содержащих специальные символы, по умолчанию имеет значение `'"'`.
- `quoting` - выделяет в символ указанный в параметре `quotechar` те элементы, которые будут указаны специальным атрибутом

Атрибуты для `quoting`:
- `QUOTE_ALL`: указывает объектам записи указывать все поля
- `QUOTE_MINIMAL`: указывает объектам записи заключать в кавычки только те поля, которые содержат специальные символы, такие как разделитель `delimiter`, кавычка `quotechar` или любой из символов в `lineterminator`
- `QUOTE_NONNUMERIC`: указывает объектам записи указывать все нечисловые поля
- `QUOTE_NONE`: указывает объектам записи никогда не заключать в кавычки поля

### Разберем на примерах:
#### Первый пример:
Рассмотрим приведенный ниже код:
```python
import csv

columns = ['first_name', 'second_name', 'class_number', 'class_letter']
data = [['Владик', 'Пантюхин', 11, 'А'], ['Владик', 'Бельский', 9, 'Б'], ['Эмиль', 'Ахметов', 10, 'В']]

with open('students.csv', 'w', encoding='utf-8', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(columns)                 # запись заголовков
    for row in data:                         # запись строк
        writer.writerow(row)
```
```
first_name,second_name,class_number,class_letter
Владик,Пантюхин,11,А
Владик,Бельский,9,Б
Эмиль,Ахметов,10,В
```
Метод `wtirows()` записывает строку с указанными разделителями и условиаями, прописанными в параметрах методов `open()` и параметрах объекта `wtriter`

Для записи вместо конструкции с циклом и методом `writerow()`, можно просто использовать метод `writerows()`, который способен запишет сразу несколько строк
То есть эти два кода делают одно и то же
```python
for row in data:
    writer.writerow(row)
```
можно написать:
```python
writer.writerows(data)
```
#### Второй пример:
Попробуем использовать больше параметров специального объекта `writer`
```python
import csv

columns = ['first_name', 'second_name', 'class_number', 'class_letter']
data = [['Владик', 'Пантюхин', 11, 'А'], ['Владик', 'Бельский', 9, 'Б'], ['Эмиль', 'Ахметов', 10, 'В']]

with open('students.csv', 'w', encoding='utf-8', newline='') as file:
    writer = csv.writer(file, delimiter=';', quoting=csv.QUOTE_NONNUMERIC)
    writer.writerow(columns)
    for row in data:
        writer.writerow(row)
```
```
"first_name";"second_name";"class_number";"class_letter"
"Владик";"Пантюхин";11;"А"
"Владик";"Бельский";9;"Б"
"Эмиль";"Ахметов";10;"В"
```
Параметр `quoting` заключит а кавычки при записи все элементы, которые соответствуют атрибуту `csv.QUOTE_NONNUMERIC`, то есть все нечисловые значения

Для записи вместо конструкции с циклом и методом `writerow()`, можно просто использовать метод `writerows()`, который способен запишет сразу несколько строк
То есть эти два кода делают одно и то же
```python
for row in data:
    writer.writerow(row)
```
можно написать:
```python
writer.writerows(data)
```