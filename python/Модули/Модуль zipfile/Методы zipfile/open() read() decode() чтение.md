#python #zip #файлы #работа_с_файлами #printdir #чтение_архива #read #open


Позволяет вытащить из архива конкретный файл.
`zip для примера`
![[test.zip]]
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    with zip_file.open('test/Разные файлы/astros.json') as file:
        print(file.read())
```
```
b'{"number": 10, "people": [{"craft": "ISS", "name": "Mark Vande Hei"}, {"craft": "ISS", "name": "Pyotr Dubrov"}, {"craft": "ISS", "name": "Thomas Pesquet"}, {"craft": "ISS", "name": "Megan McArthur"}, {"craft": "ISS", "name": "Shane Kimbrough"}, {"craft": "ISS", "name": "Akihiko Hoshide"}, {"craft": "ISS", "name": "Anton Shkaplerov"}, {"craft": "Shenzhou 13", "name": "Zhai Zhigang"}, {"craft": "Shenzhou 13", "name": "Wang Yaping"}, {"craft": "Shenzhou 13", "name": "Ye Guangfu"}], "message": "success"}'
```
Метод `ZipFile.open()` открывает файл именно в бинарном виде, а не в текстовом.

Перед выводом печатается символ `b`.  Потому что, вообще это бинарная строка. Метод `file.read()` возвращает сырые байты(тип `bytes`). Для того, чтобы преобразовать их в строку`str`, нужно использовать метод `decode()`, указав кодировку. Файл `astros.json` имеет кодировку `UTF-8`
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    with zip_file.open('test/Разные файлы/astros.json') as file:
        print(file.read().decode('utf-8'))
```
```
{"number": 10, "people": [{"craft": "ISS", "name": "Mark Vande Hei"}, {"craft": "ISS", "name": "Pyotr Dubrov"}, {"craft": "ISS", "name": "Thomas Pesquet"}, {"craft": "ISS", "name": "Megan McArthur"}, {"craft": "ISS", "name": "Shane Kimbrough"}, {"craft": "ISS", "name": "Akihiko Hoshide"}, {"craft": "ISS", "name": "Anton Shkaplerov"}, {"craft": "Shenzhou 13", "name": "Zhai Zhigang"}, {"craft": "Shenzhou 13", "name": "Wang Yaping"}, {"craft": "Shenzhou 13", "name": "Ye Guangfu"}], "message": "success"}
```
Символа `b` в начале строки в этот раз уже нет