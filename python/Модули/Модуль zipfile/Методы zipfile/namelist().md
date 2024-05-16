#python #zip #файлы #работа_с_файлами #infolist #чтение_архива #namelist


Возвращает список названий файлов и директорий, содержащихся в архиве.
`zip для примера`
![[test.zip]]
```python
from zipfile import ZipFile

with ZipFile('test.zip') as zip_file:
    info = zip_file.namelist()
    print(*info, sep='\n')
```
```
test/
test/Картинки/
test/Картинки/1.jpg
test/Картинки/avatar.png
test/Картинки/certificate.png
test/Картинки/py.png
test/Картинки/World_Time_Zones_Map.png
test/Картинки/Снимок экрана.png
test/Неравенства.djvu
test/Программы/
test/Программы/image_util.py
test/Программы/sort.py
test/Разные файлы/
test/Разные файлы/astros.json
```