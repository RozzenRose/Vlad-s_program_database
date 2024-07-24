#метод #itertools #python #groupbi


Функция `groupby()` используется для группировки одинаковых элементов итерируемого объекта. Она возвращает итератор, содержащий кортежи, каждый из которых состоит из двух элементов: первый — значение, характеризующее группу, второй — итератор, содержащий элементы соответствующей группы.

Аргументы функции:
- `iterable` — итерируемый объект
- `key` — функция, вычисляющая значение, характеризующее группу

Если `key` не указан, ключом по умолчанию является функция тождественности, которая возвращает элемент без изменений
```python
from itertools import groupby

numbers = [1, 1, 1, 7, 7, 7, 7, 15, 7, 7, 7]

group_iter = groupby(numbers)

print(type(group_iter))
print(*group_iter, sep='\n')
```
```
<class 'itertools.groupby'>
(1, <itertools._grouper object at 0x0000022424FCA590>)
(7, <itertools._grouper object at 0x0000022424FCA410>)
(15, <itertools._grouper object at 0x0000022424FCB2B0>)
(7, <itertools._grouper object at 0x0000022424FCBBB0>)
```

Не только ответ функции `groupby()` является итератором, но и вторые элементы в кортежах ответа тоже являются итераторами, а значит для их распаковки мы можем использовать любой удобный для нас метод, например преобразовать их в списки:
```python
from itertools import groupby

numbers = [1, 1, 1, 7, 7, 7, 7, 15, 7, 7, 7]

group_iter = groupby(numbers)

for key, values in group_iter:
    print(f'{key}: {list(values)}')            # преобразуем итератор в список
```
```
1: [1, 1, 1]
7: [7, 7, 7, 7]
15: [15]
7: [7, 7, 7]
```

Вот так это выглядит в схематическом виде:
![[Pasted image 20240722154548.png]]Стоит обратить внимание, что список `numbers` не был отсортирован перед передачей его в функцию `groupby()`, именно по этой причине мы получили две группы, содержащие цифры `7`, если сначала отсортировать список, мы получим одну большую группу с цифрами `7`

```python
from itertools import groupby

numbers = [1, 1, 1, 7, 7, 7, 7, 15, 7, 7, 7]

group_iter = groupby(sorted(numbers))

for key, values in group_iter:
    print(f'{key}: {list(values)}')            # преобразуем итератор в список
```
```
1: [1, 1, 1]
7: [7, 7, 7, 7, 7, 7, 7]
15: [15]
```

#### Использование аргумента `key`
Позволяет совершить группировку по ответам переданной в `key` функции:```python
```python
from itertools import groupby

numbers = [1, 1, 1, 7, 7, 7, 7, 15, 7, 7, 7]

group_iter = groupby(numbers, key=lambda num: num < 10)

for key, values in group_iter:
    print(f'{key}: {list(values)}')
```
```
True: [1, 1, 1, 7, 7, 7, 7]
False: [15]
True: [7, 7, 7]
```
Таким образом, мы разбили итерируемый объект на группы, для одних из которых выполняется условие внутри `lambda` функции, а для других нет

Для того чтобы не получать после применения `groupby()` повторяющиеся группы, нам как и прежде нужно сперва отсортировать итерируемый объект. НО сортировать его можно той же самой функцией, которую мы используем для создания группировок! 
```python
from itertools import groupby

numbers = [1, 1, 1, 7, 7, 7, 7, 15, 7, 7, 7]

key_func = lambda num: num < 10

group_iter = groupby(sorted(numbers, key=key_func), key=key_func)

for key, values in group_iter:
    print(f'{key}: {list(values)}')
```
```
False: [15]
True: [1, 1, 1, 7, 7, 7, 7, 7, 7, 7]
```


### Примеры
#### Пример 1
Удалить подряд идущие одинаковые элементы в списке.
```python
from itertools import groupby

data = ['A', 'A', 'A', 'A', 'B', 'B', 'B', 'C', 'C', 'D', 'A', 'A', 'B', 'B', 'B']

result = [key for key, group in groupby(data)] 

print(result)
```
```
['A', 'B', 'C', 'D', 'A', 'B']
```

#### Пример 2
Получить список с уникальными элементами списка.
```python
from itertools import groupby

data = ['A', 'A', 'A', 'A', 'B', 'B', 'B', 'C', 'C', 'D', 'A', 'A', 'B', 'B', 'B']

result = [key for key, group in groupby(sorted(data))] 

print(result)

```
```
['A', 'B', 'C', 'D']
```

#### Пример 3
Определить, какой символ встречается чаще всего в строке.
```python
from itertools import groupby

data = 'aaabcdaabcccdddcccccccbrttbcc'
group_iter = groupby(sorted(data))

max_result = max(group_iter, key=lambda tpl: sum(1 for i in tpl[1]))

print('Символ встречающийся чаще всего в строке:', max_result[0])
```
```
Символ встречающийся чаще всего в строке: c
```
После применения функции `max` итератор `group_iter` становится пустым!

Если ты планируешь использовать эти данные повторно, имеет смысл их сохранить:
```python
from itertools import groupby

data = 'aaabcdaabcccdddcccccccbrttbcc'
group_iter = groupby(sorted(data))

saved_data = [(key, list(group)) for key, group in group_iter]

max_result = max(saved_data, key=lambda tpl: len(tpl[1]))       # можем уже использовать len()
min_result = min(saved_data, key=lambda tpl: len(tpl[1]))

print('Символ встречающийся чаще всего в строке:', max_result[0])
print('Символ встречающийся реже всего в строке:', min_result[0])
```

### Примечание:
Эта штука ведет себя немного неочевидно при работе с итераторами: 
```python
from itertools import groupby

iterable = iter('aaabbbccc')
groups = groupby(iterable)

key1, group1 = next(groups)

print(list(iterable))  # ['a', 'a', 'b', 'b', 'b', 'c', 'c', 'c']
```
Из исходного итератора пропадает только один элемент, функция считала, его для определения ключа по сути, а переменная `group1` вообще то ссылается на тот же самый объект, что и `iterable`, то есть на исходный итератор! Но через `next()` или преобразование в другой тип, можно вызвать только первые буквы `a`, но если это сделать, то они пропадут их исходного итератора, потому что были считаны:
```python
from itertools import groupby

iterable = iter('aaabbbccc')
groups = groupby(iterable)

key1, group1 = next(groups)

print(list(group1))    # ['a', 'a', 'a']
print(list(iterable))  # ['b', 'b', 'c', 'c', 'c']
```
Вроде хуйня, но, если ты применишь `next()` к итератору, который вернул `groupby()` два раза, то итератор, который ты получил в ответе в первый раз окажется пуст!
```python
from itertools import groupby

iterable = iter('aaabbbccc')
groups = groupby(iterable)

key1, group1 = next(groups)
key2, group2 = next(groups)

print(list(group1))    # []
print(list(iterable))  # ['b', 'b', 'c', 'c', 'c']
```
Потому что итераторы в ответе `groupby()` ссылаются на тот же самый объект, который мы присваивали переменной `iterable`. Вторым вызовом итератора из `groupby()` мы по сути вызываем уже первую букву `b`, а значит, все что было до этой буквы больше не существует в этом объекте!
