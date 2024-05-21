#python #файлы #работа_с_файлами #python #json #сериализация #dumps #метод

Метод предназначенный для сериализации данных данных в формат `json`.
Вообще `json` - это текстовый формат, а это значит, что сериализация в него, это вообще перевод данных в строку. 
```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data = json.dumps(data)            # сериализуем словарь data в json строку

print(type(json_data))
print(json_data)
```
```
<class 'str'>
{"name": "Russia", "phone_code": 7, "capital": "Moscow", "currency": "RUB"}
```

#### skipkeys
С помощью этого аргумента можно игнорировать ключи, которые при попытки преобразования в `json` вызовут ошибки.
```python
import json

data = {
        'beegeek': 2018,
        ('VlaDICK', 'Pantykhin'): 29,
        ('Wlad', 'Belsky'): 20,
        'stepik': 2013
       }

json_data = json.dumps(data, skipkeys=True)        # преобразуем dict в json

print(json_data)
```
```json
{"beegeek": 2018, "stepik": 2013}
```

#### ensure_ascii
Отменяет кодирование кириллических символов при сериализации
```python
import json

data = {'firstName': 'Владик', 'lastName': 'Пантюхин'}
s = json.dumps(data, ensure_ascii=False)
print(s)
result = json.loads(s)
print(result)
```
```
{"firstName": "Владик", "lastName": "Пантюхин"}
{'firstName': 'Владик', 'lastName': 'Пантюхин'}
```

#### indent отступ
Задает отступ от левого края. по дефолту имеет значение `None` для более компактного представления отступов.
```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data1 = json.dumps(data, indent=2)
json_data2 = json.dumps(data, indent=10)

print(json_data1)
print(json_data2)
```
```
{
  "name": "Russia",
  "phone_code": 7,
  "capital": "Moscow",
  "currency": "RUB"
}
{
          "name": "Russia",
          "phone_code": 7,
          "capital": "Moscow",
          "currency": "RUB"
}
```
Если а значение переданное в `indent` является числовым, при переводе в `json` просто будет добавлен отступ в виде количестве пробелов, соответствующего переданному числу. Но вообще-то туда можно передать еще и строку, и тогда отступ будет состоять из этой строки.
```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}
json_data = json.dumps(data, indent='++++')

print(json_data)
```
```
{
++++"name": "Russia",
++++"phone_code": 7,
++++"capital": "Moscow",
++++"currency": "RUB"
}
```
#### sort_keys сортировка ключей
Задает сортировку ключей в результирующем `json`. По умолчанию имеет значение `False`.
Если установить значение `True`, то ключи будут отсортированы в алфавитном порядке.
```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data1 = json.dumps(data, indent=3)
json_data2 = json.dumps(data, indent=3, sort_keys=True)

print(json_data1)
print(json_data2)
```
```json
{
   "name": "Russia",
   "phone_code": 7,
   "capital": "Moscow",
   "currency": "RUB"
}
{
   "capital": "Moscow",
   "currency": "RUB",
   "name": "Russia",
   "phone_code": 7
}
```

#### separators разделители
В аргумент `separators` задается кортеж, состоящий из двух элементов `(item_separator, key_separator)`, которые представляют разделители для элементов и ключей. По дефолту значения следующие `(', ', ': ')`.
```python
import json

data = {'name': 'Russia', 'phone_code': 7, 'capital': 'Moscow', 'currency': 'RUB'}

json_data = json.dumps(data, indent=3, separators=(';', ' = '))

print(json_data)
```
```json
{
   "name" = "Russia";
   "phone_code" = 7;
   "capital" = "Moscow";
   "currency" = "RUB"
}
```
