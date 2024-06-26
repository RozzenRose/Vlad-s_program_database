#python #next #встроенная_функция #метод

Принимает в качестве аргумента итератор, и возвращает элемент из этого итератора с наименьшим индексом. При этом этот элемент будет удален из итератора. При следующем вызове функция `next()` вернет следующий элемент и т.д. Если в итераторе не осталось элементов, то вызов функции `next()` приведет к возникновению исключения `StopIteration`
```python
numbers = [1, 2, 3]

iterator = iter(numbers)          # создаем итератор на основе списка

print(next(iterator))             # запрашиваем и печатаем первый элемент итератора
print(next(iterator))             # запрашиваем и печатаем второй элемент итератора
print(next(iterator))             # запрашиваем и печатаем третий элемент итератора

print(next(iterator))             # возбуждается исключение StopIteration
```
Можно передать второй позиционный аргумент, который будет возвращен вместо возбуждения исключения `StopIteration`, если в итераторе больше не осталось  элементов
```python
nums = iter([1, 2, 3, 4])

print(next(nums))
print(next(nums))
print(next(nums))
print(next(nums))
print(next(nums, -1))
print(next(nums, -1))
```
```
1
2
3
4
-1
-1
```