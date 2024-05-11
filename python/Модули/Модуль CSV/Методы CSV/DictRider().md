#python #файлы #работа_с_файлами #python #CSV #TSV #DictRider #специальный_обект #CSV_dict #DictReader #DictWriter


Специальный объект `DictRider` является частью [[Модуль CSV|модуля CSV]]. И делится в свою очередь на два подобъекта. `DictWriter` и `DictReader`

`DictReader` поддерживает создание объекта-словаря на основе названий столбцов. С помощью `DictReader` мы можем обращаться к полям не по индексу, а по названию. Это делает код более читаемым.
```python
import csv
rows = csv.DictReader(file, delimiter=';', quotechar='"', fieldnames=fieldnames)
```
- `file` - название файла
- `delimiter` - межэлементный разделитель, по умолчанию `','`
- `quotechar` — односимвольная строка, используемая для кавычек в полях, содержащих специальные символы, по умолчанию имеет значение `'"'`.
- `fildnames` - можно самостоятельно задать заголовки для столбцов, если те, которые есть в файле тебя не устраивают

`DictWriter` позволяет писать содержимое словарей в файлы в формате `CSV`
```python
import csv
writer = csv.DictWriter(file, fieldnames=, delimiter=';', quotechar='"', quoting=)
```
- `file` - название файла
- `delimiter` - межэлементный разделитель, по умолчанию `','`
- `quotechar` — односимвольная строка, используемая для кавычек в полях, содержащих специальные символы, по умолчанию имеет значение `'"'`.
- `fieldnames` - можно самостоятельно задать заголовки для столбцов, если те, которые есть в файле тебя не устраивают
- `quoting` - выделяет в символ указанный в параметре `quotechar` те элементы, которые будут указаны специальным атрибутом

Атрибуты для `quoting`:
- `QUOTE_ALL`: указывает объектам записи указывать все поля
- `QUOTE_MINIMAL`: указывает объектам записи заключать в кавычки только те поля, которые содержат специальные символы, такие как разделитель `delimiter`, кавычка `quotechar` или любой из символов в `lineterminator`
- `QUOTE_NONNUMERIC`: указывает объектам записи указывать все нечисловые поля
- `QUOTE_NONE`: указывает объектам записи никогда не заключать в кавычки поля

### Примеры:
#### Примеры чтения:
##### Первый пример чтения:
Допустим у нас есть вот такой вот файл `products.csv` (с разделителем `';'`)
```
keywords;price;product_name
"Садовый стул, стул для дачи";1699;ВЭДДО
Садовый стул;2999;ЭПЛАРО
Садовый табурет;1699;ЭПЛАРО
Садовый стол;1999;ТЭРНО
"Складной стол, обеденный стол";7499;ЭПЛАРО
Настил;1299;РУННЕН
Стеллаж;1299;ХИЛЛИС
"Кружка, сосуд, стакан с ручкой";39;СТЕЛЬНА
Молочник;299;ВАРДАГЕН
Термос для еды;699;ЭФТЕРФРОГАД
Ситечко;59;ИДЕАЛИСК
Чайник заварочный;499;РИКЛИГ
Кофе-пресс;699;УПХЕТТА
Чашка с блюдцем;249;ИКЕА
"Кружка, стакан с ручкой";249;ЭМНТ
Ситечко;199;САККУННИГ
Кружка;199;ФИНСТИЛТ
"Тарелка, блюдце";269;ЭВЕРЕНС
```
Применим у нему следующий код:
```python
import csv

with open('products.csv', encoding='utf-8') as file:
    rows = csv.DictReader(file, delimiter=';', quotechar='"')
    for row in rows:
        print(row)
```
```
{'keywords': 'Садовый стул, стул для дачи', 'price': '1699', 'product_name': 'ВЭДДО'}
{'keywords': 'Садовый стул', 'price': '2999', 'product_name': 'ЭПЛАРО'}
{'keywords': 'Садовый табурет', 'price': '1699', 'product_name': 'ЭПЛАРО'}
{'keywords': 'Садовый стол', 'price': '1999', 'product_name': 'ТЭРНО'}
{'keywords': 'Складной стол, обеденный стол', 'price': '7499', 'product_name': 'ЭПЛАРО'}
{'keywords': 'Настил', 'price': '1299', 'product_name': 'РУННЕН'}
{'keywords': 'Стеллаж', 'price': '1299', 'product_name': 'ХИЛЛИС'}
{'keywords': 'Кружка, сосуд, стакан с ручкой', 'price': '39', 'product_name': 'СТЕЛЬНА'}
{'keywords': 'Молочник', 'price': '299', 'product_name': 'ВАРДАГЕН'}
{'keywords': 'Термос для еды', 'price': '699', 'product_name': 'ЭФТЕРФРОГАД'}
{'keywords': 'Ситечко', 'price': '59', 'product_name': 'ИДЕАЛИСК'}
{'keywords': 'Чайник заварочный', 'price': '499', 'product_name': 'РИКЛИГ'}
{'keywords': 'Кофе-пресс', 'price': '699', 'product_name': 'УПХЕТТА'}
{'keywords': 'Чашка с блюдцем', 'price': '249', 'product_name': 'ИКЕА'}
{'keywords': 'Кружка, стакан с ручкой', 'price': '249', 'product_name': 'ЭМНТ'}
{'keywords': 'Ситечко', 'price': '199', 'product_name': 'САККУННИГ'}
{'keywords': 'Кружка', 'price': '199', 'product_name': 'ФИНСТИЛТ'}
{'keywords': 'Тарелка, блюдце', 'price': '269', 'product_name': 'ЭВЕРЕНС'}
```
В переменную `rows` возвращается итератор, содержащий словари, каждый из которых содержит все элементы, одной строки, а ключами к значениям этих элементов являются соответствующие заголовки столбцов.


То-есть каждый цикл в переменную `row` присваивается словарь.

##### Второй пример чтения:
Вытащим к примеру 5 самых дорогих товаров:
```python
import csv

with open('products.csv', encoding='utf-8') as file:
    rows = csv.DictReader(file, delimiter=';', quotechar='"')
    expensive = sorted(rows, key=lambda item: int(item['price']), reverse=True)
    for record in expensive[:5]:
        print(record)
```
```
{'keywords': 'Складной стол, обеденный стол', 'price': '7499', 'product_name': 'ЭПЛАРО'}
{'keywords': 'Садовый стул', 'price': '2999', 'product_name': 'ЭПЛАРО'}
{'keywords': 'Садовый стол', 'price': '1999', 'product_name': 'ТЭРНО'}
{'keywords': 'Садовый стул, стул для дачи', 'price': '1699', 'product_name': 'ВЭДДО'}
{'keywords': 'Садовый табурет', 'price': '1699', 'product_name': 'ЭПЛАРО'}
```

Так же мы можем заменить ключи в словарях на этапе формирования словаря при помощи атрибута `fildnames`:
```python
import csv
# Определяем fieldnames вручную
fieldnames = ['id', 'first_name', 'last_name']
# Используем DictReader для чтения данных
with open('products.csv', encoding='utf-8') as file:
    rows = csv.DictReader(file, fieldnames=fieldnames)
    for row in rows:
        print(row)
```
```
{'id': 'Садовый стул, стул для дачи', 'first_name': '1699', 'last_name': 'ВЭДДО'}
{'id': 'Садовый стул', 'first_name': '2999', 'last_name': 'ЭПЛАРО'}
{'id': 'Садовый табурет', 'first_name': '1699', 'last_name': 'ЭПЛАРО'}
{'id': 'Садовый стол', 'first_name': '1999', 'last_name': 'ТЭРНО'}
{'id': 'Складной стол, обеденный стол', 'first_name': '7499', 'last_name': 'ЭПЛАРО'}
{'id': 'Настил', 'first_name': '1299', 'last_name': 'РУННЕН'}
{'id': 'Стеллаж', 'first_name': '1299', 'last_name': 'ХИЛЛИС'}
{'id': 'Кружка, сосуд, стакан с ручкой', 'first_name': '39', 'last_name': 'СТЕЛЬНА'}
{'id': 'Молочник', 'first_name': '299', 'last_name': 'ВАРДАГЕН'}
{'id': 'Термос для еды', 'first_name': '699', 'last_name': 'ЭФТЕРФРОГАД'}
{'id': 'Ситечко', 'first_name': '59', 'last_name': 'ИДЕАЛИСК'}
{'id': 'Чайник заварочный', 'first_name': '499', 'last_name': 'РИКЛИГ'}
{'id': 'Кофе-пресс', 'first_name': '699', 'last_name': 'УПХЕТТА'}
{'id': 'Чашка с блюдцем', 'first_name': '249', 'last_name': 'ИКЕА'}
{'id': 'Кружка, стакан с ручкой', 'first_name': '249', 'last_name': 'ЭМНТ'}
{'id': 'Ситечко', 'first_name': '199', 'last_name': 'САККУННИГ'}
{'id': 'Кружка', 'first_name': '199', 'last_name': 'ФИНСТИЛТ'}
{'id': 'Тарелка, блюдце', 'first_name': '269', 'last_name': 'ЭВЕРЕНС'}
```

#### Пример записи:
Рассмотрим приведенный ниже код:
```python
import csv

data = [{'first_name': 'Владик', 'second_name': 'Пантюхин', 'class_number': 11, 'class_letter': 'А'},
        {'first_name': 'Андрей', 'second_name': 'Юрик', 'class_number': 9, 'class_letter': 'Б'},
        {'first_name': 'Ден', 'second_name': 'Фельдшер', 'class_number': 10, 'class_letter': 'В'}]

columns = ['first_name', 'second_name', 'class_number', 'class_letter']

with open('students.csv', 'w', encoding='utf-8', newline='') as file:
    writer = csv.DictWriter(file, fieldnames=columns, delimiter=';', quoting=csv.QUOTE_NONNUMERIC)
    writer.writeheader()                 # запись заголовков
    for row in data:                     # запись строк
        writer.writerow(row)
```
```
"first_name";"second_name";"class_number";"class_letter"
"Владик";"Пантюхин";11;"А"
"Андрей";"Юрик";9;"Б"
"Ден";"Фельдшер";10;"В"
```
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