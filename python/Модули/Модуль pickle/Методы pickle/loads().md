#python #файлы #работа_с_файлами #python #json #десериализация #pickle #метод #loads


Принимает объекты типа `bytes`, и десерализириализирует их в объекты `python`.
```python
import pickle

obj = {'Python': 1991, 'Java': 1995, 'C#': 2002}
binary_obj = pickle.dumps(obj)

new_obj = pickle.loads(binary_obj)

print(new_obj)
```
```no-highlight
{'Python': 1991, 'Java': 1995, 'C#': 2002}
```