#python #файлы #работа_с_файлами #python #json #десериализация #pickle #метод #load


Метод принимает на вход файловый объект , полученный на основе файла с расширением `.pkl`. После чего данные из файла будут десериализированы в `Python` объект. 
```python
import pickle

with open('file.pkl', 'rb') as file:     # используется файл полученный на предыдущем шаге
    obj = pickle.load(file)
    print(obj)
    print(type(obj))
```
```
{'Python': 1991, 'Java': 1995, 'C#': 2002}
<class 'dict'>
```

А этих файлах можно хранить целые функции, допустим у нас есть вот такая функция
```python
def func(*args):
    return ' '.join(args)
```
Но запакована она в файл `file.pkl`
```python
args = ['Hello,', 'how', 'are', 'you', 'today?']

with open(file, 'rb') as pkl_file:
    func = pickle.load(pkl_file)
    print(func(*args))
```
```
Hello, how are you today?
```