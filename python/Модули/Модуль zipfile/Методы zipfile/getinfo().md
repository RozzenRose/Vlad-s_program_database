#python #zip #файлы #работа_с_файлами #infolist #чтение_архива #getinfo #метод


Метод позволяет получить информацию о конкретном файле по его имени в архиве.
`zip для примера`
![[test.zip]]
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    last_file = zip_file.getinfo('image_util.py')    # получаем информацию об отдельном файле
    print(last_file.file_size)
    print(last_file.compress_size)
    print(last_file.filename)
    print(last_file.date_time)
```
```
4955
1641
test/Программы/image_util.py
(2021, 11, 18, 12, 42, 22)
```
