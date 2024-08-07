#метод #itertools #python #tee


Функция `tee()` позволяет создать несколько независимых итераторов на основе одного и того же итерируемого объекта

Аргументы функции:
- `iterable` — итерируемый объект
- `n` — количество создаваемых итераторов, по умолчанию имеет значение 2

Итераторы, возвращаемые функцией `tee()`, могут быть использованы с целью передачи одного и того же набора данных нескольким алгоритмам для их параллельной обработки
```python
iter1, iter2 = tee([1, 'a', 2, 'b', 3, 'c'])    # по умолчанию n=2

print(*iter1)
print(*iter2)
```
```
1 a 2 b 3 c
1 a 2 b 3 c
```
Кстати, возвращает объект типа `tuple`

```python
from itertools import tee

result = tee(range(2, 7), 5)
print(type(result))
for i in result:
    print(*i)
```
```
<class 'tuple'>               # функция tee() возвращает кортеж с нужными итераторами
2 3 4 5 6
2 3 4 5 6
2 3 4 5 6
2 3 4 5 6
2 3 4 5 6
```
Семантика функции `tee()` аналогична семантике утилиты `tee()` в `Unix`, которая повторяет значения, читаемые их входного потока, и записывает их в именованный файл или стандартный вывод

Новые итераторы, созданные функцией `tee()`, разделяют данные с исходным итерируемым объектом, и поэтому после их создания исходный итерируемый объект не должен изменяться
```python
from itertools import tee

data = (i for i in range(10))     # используем генераторное выражение

iter1, iter2 = tee(data)

print(next(data))                 # изменяем data                        

print(*iter1)
print(*iter2)
```
```
0
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
```
Если исходный итерируемый объект является итератором, то значения, уже обработанные им ранее, не будут возвращаться вновь созданными итераторами.

Аналогичный результат получим, если в качестве итерируемого объекта передать список, а затем его изменить
```python
from itertools import tee

data = [1, 2, 3, 4, 5]

iter1, iter2 = tee(data)

data.append(6)                      

print(*iter1)
print(*iter2)
```
```
1 2 3 4 5 6
1 2 3 4 5 6
```
Для выполнения функции `tee()` может потребоваться значительное количество дополнительной памяти, в зависимости от того, сколько данных необходимо сохранить