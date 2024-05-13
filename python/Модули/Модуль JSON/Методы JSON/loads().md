#python #файлы #работа_с_файлами #python #json #десериализация #loads


Метод используется для десереализации данных. В метод просто передается строка в формате #json 
```python
import json

json_data = '{"name": "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}'

data = json.loads(json_data)
print(type(data))
print(data)
```
```
<class 'dict'>
{'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}
```
Если данные в строке будут содержать ошибку, программа завершит свое выполнение с ошибкой `json.decoder.JSONDecodeError`

Метод возвращает объект типа `dict`