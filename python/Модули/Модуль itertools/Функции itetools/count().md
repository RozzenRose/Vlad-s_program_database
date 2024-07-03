#метод #itertools #python #count 

Функция `count()` возвращает итератор, генерирующий бесконечную последовательность чисел
Аргументы функции:
- `start` — начало отсчета, по умолчанию имеет значение 0
- `step` — шаг, по умолчанию имеет значение 1

В отличии от встроенной функции `range()` , в `count()` аргумент для задания верхней границы не предусмотрен
```python
from itertools import count

count1 = count()

print(next(count1))
print(next(count1), next(count1), next(count1))

count2 = count(69, 10)

print(next(count2))
print(next(count2))
print(next(count2), next(count2), next(count2))

for i in zip(count(10), ['a', 'b', 'c']):
    print(i)
```
```
0
1 2 3
69
79
89 99 109
(10, 'a')
(11, 'b')
(12, 'c')
```
На основе такого итератора, мы не можем создавать конечные последовательности например объект типа `list`

Аргумент `start` и `step` функции `count()` могут быть любые числовые значения, допускающие операцию сложения
```python
from itertools import count
from fractions import Fraction

for index, number in enumerate(count(1.0, 0.5)):
    if index < 6:
        print(number)
    else:
        break

frac_iter = count(1, Fraction(1, 2))
print(next(frac_iter), next(frac_iter), next(frac_iter), next(frac_iter), next(frac_iter))
```
```
1.0
1.5
2.0
2.5
3.0
3.5
1 3/2 2 5/2 3
```