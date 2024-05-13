#python #json #файлы #работа_с_файлами #сериализация #десериализация 


`json` может содержать следующие типы данных:
- `str` - строка
- `int` - целочисленные значения
- `float` - значения с плавающей точкой
- `list` - список
- `dict` - словарь

Модуль `json` автоматически определяет тип значения при десериализации:
```python
import json

json_data = '''
{
   "name": "Russia",
   "phone_code": 7,
   "latitude": 60.0,
   "capital": "Moscow",
   "timezones": ["Anadyr", "Barnaul", "Moscow", "Kirov"],
   "translations": {
      "nl": "Rusland",
      "hr": "Rusija",
      "de": "Russland",
      "es": "Rusia",
      "fr": "Russie",
      "it": "Russia"
   }
}'''

data = json.loads(json_data)

print(type(data['name']))
print(type(data['phone_code']))
print(type(data['latitude']))
print(type(data['timezones']))
print(type(data['translations']))
```
```
<class 'str'>
<class 'int'>
<class 'float'>
<class 'list'>
<class 'dict'>
```
### Автоматическое изменение формата данных и исключения
При сериализации данных их тип может быть изменен. В случае если мы пытаемся сериализировать тип данных, которого нет в `json`, модуль `json` сам преобразует эти данные в логически близки тип данных. Например тип `tuple` будет преобразован в `list`

Таблица конвертации типов данных Python в JSON:

|Python|JSON|
|---|---|
|`dict`|`object`|
|`list, tuple`|`array`|
|`str`|`string`|
|`int, float`|`number`|
|`True`|`true`|
|`False`|`false`|
|`None`|`null`|

Таблица конвертации JSON в типы данных Python:

| JSON            | Python  |
| --------------- | ------- |
| `object`        | `dict`  |
| `array`         | `list`  |
| `string`        | `str`   |
| `number (int)`  | `int`   |
| `number (real)` | `float` |
| `true`          | `True`  |
| `false`         | `False` |
| `null`          | `None`  |
Итак мы можем сериализировать любой тип данных, поддерживаемый `json`
```python
import json

colors = ['white', 'red', 'black']

with open('list.json', 'w') as file:
    json.dump(colors, file, indent='---')
```
создает файл `list.json` со следующим содержанием:
```
[
---"white",
---"red",
---"black"
]
```

Пример изменения типы данных при сериализации/десериализации
```python
import json

data = {
        'name': 'Russia', 
        'phone_code': 7,
        'latitude': 60.0,
        'capital': 'Moscow',
        'timezones': ('Anadyr', 'Barnaul', 'Moscow', 'Kirov')
       }

json_data = json.dumps(data)        # преобразуем dict в json
new_data = json.loads(json_data)    # преобразуем json в dict

print(data == new_data)
```
```python
False
```

В формат `json` нельзя перевести словарь, ключами которого являются кортежи:
```python
import json

data = {
        'beegeek': 2018,
        ('VlaDICK', 'Pantykhin'): 29,
        ('Wlad', 'Belsky'): 20,
        'stepik': 2013
       }

json_data = json.dumps(data)        # преобразуем dict в json

print(json_data)
```
```no-highlight
TypeError: keys must be str, int, float, bool or None, not tuple
```
При помощи параметра `skipkeys` методов `dump()` и `dumps()`можно игнорировать сериализацию данных, которые подписаны непонятными для `json` ключами.

Вообще ключами для словарей в `json` могут быть только строки, но если мы попробуем засунуть в `json` словарь с ключами из значений типа `int`, то все эти ключи будут автоматически преобразованы в строки:
```python
import json

data = {1: 'Timur', False: 'Arthur', None: 'Ruslan'}
json_data = json.dumps(data)

print(json_data)
```
```json
{"1": "Timur", "false": "Arthur", "null": "Ruslan"}
```