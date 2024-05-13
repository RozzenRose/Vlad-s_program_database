#python #json #файлы #работа_с_файлами #сериализация #десериализация 


Кириллические символы при сериализации заменяются на свой уникальный код. Эти коды стандартны и уникальны для каждого символа. И вообще формат кодировки `C/C++/Java source code`
```python
import json 

data = {'firstName': 'Владик', 'lastName': 'Пантюхин'} 
s = json.dumps(data) 
print(s) result = json.loads(s) 
print(result)
```
```
{"firstName": "\u0412\u043b\u0430\u0434\u0438\u043a", "lastName": "\u041f\u0430\u043d\u0442\u044e\u0445\u0438\u043d"}
{'firstName': 'Владик', 'lastName': 'Пантюхин'}
```
При десериализации данные вернутся в исходный вид.

Но при помощи параметра `ensure_ascii` методов `dumps()` и `dump()` можно отказаться от такого кодирования и данные будет сериализироваться в исходном виде.
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

