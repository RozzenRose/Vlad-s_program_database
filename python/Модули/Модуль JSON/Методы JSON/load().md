#python #файлы #работа_с_файлами #python #json #десериализация #load


В отличии от метода `loads()`, которая в качестве аргумента принимает строку с данными в формате `json`, метод `load()` в качестве аргумента принимает файл формата `.json` и возвращает его десериализированное содержимое.

Допустим у нас есть вот такой вот файлик вод названием `date.json`:
```json
{
  "name": "Russia",
  "phone_code": 7,
  "capital": "Moscow",
  "cities": ["Abakan", "Almetyevsk", "Anadyr", "Anapa", "Arkhangelsk", "Astrakhan"],
  "currency": "RUB"
}
```
Запускаем следующий код:
```python
import json

with open('data.json') as file:
    data = json.load(file)                # передаем файловый объект
    for key, value in data.items():
        if type(value) == list:
            print(f'{key}: {", ".join(value)}')
        else:
            print(f'{key}: {value}')
```
```
name: Russia
phone_code: 7
capital: Moscow
cities: Abakan, Almetyevsk, Anadyr, Anapa, Arkhangelsk, Astrakhan
currency: RUB
```
