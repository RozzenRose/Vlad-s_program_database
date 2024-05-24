#python #файлы #работа_с_файлами #python#сериализация #pickle #метод 
#dumps


Метод принимает любой `python` объект, сериализирует его в бинарный вид, вернет экземпляр класса `bytes`
```python
import pickle

obj = {'Python': 1991, 'Java': 1995, 'C#': 2002}
binary_obj = pickle.dumps(obj)

print(binary_obj)
print(type(binary_obj))
```
```
b'\x80\x03}q\x00(X\x06\x00\x00\x00Pythonq\x01M\xc7\x07X\x04\x00\x00\x00Javaq\x02M\xcb\x07X\x02\x00\x00\x00C#q\x03M\xd2\x07u.'
<class 'bytes'>
```
