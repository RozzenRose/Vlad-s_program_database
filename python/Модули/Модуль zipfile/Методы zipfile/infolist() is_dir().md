#python #zip #файлы #работа_с_файлами #infolist #чтение_архива #zipfile #is_dir


Позволяет получить информацию о файлах из архива в виде списка специальных объектов, которые содержат дополнительную информацию о каждом файле:
- `file_size`
- `compress_size`
- `filename`
- `date_time`
- ...
`zip для примера`
![[test.zip]]
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    info = zip_file.infolist()
    print(info[6].file_size)                # размер начального файла в байтах
    print(info[6].compress_size)            # размер сжатого файла в байтах
    print(info[6].filename)                 # имя файла
    print(info[6].date_time)                # дата изменения файла
```
```
2324421
2322032
test/Картинки/World_Time_Zones_Map.png
(2021, 11, 8, 7, 30, 6)
```
Это самые базовые атрибуты.

Интересно, что атрибут `date_time` возвращает `tuple(год, месяц, день, час, минута, секунда)`, ну то-есть если подкинуть модуль `datetime`, и закинуть в конструктор `datetime` объектов этот атрибут, то мы сразу получаем объект типа `datetime` на основе последней даты изменения файла.

### is_dir()
Подкласс метода `infolist()`, который используется для определение является ли объект папкой: Он возвращает `True`, если объект является папкой и `False` в противном случае.
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    info = zip_file.infolist()
    print(info[0].is_dir())
    print(info[6].is_dir())
```
```
True
False
```