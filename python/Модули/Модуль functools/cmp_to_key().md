#functools #сортировка #python #cmp_to_key

Очень хитрый и мощный инструмент сортировки. присваивается параметру `key`, в методах сортировки типа `sorted()` или `sort()`

Требует подкинуть [[Модуль funсtools]]

Допустим нужно расположить цифры в списке, таким образом, что бы их расположение давало наибольшее число, в случае, если их склеить как бы
```python
from functools import cmp_to_key

def compare(x, y):
    if x + y > y + x:
        return 1
    elif x + y < y + x:
        return -1
    else:
        return 0

def get_biggest(numbers):
    if not numbers:
        return -1
    # Преобразование чисел в строки для последующего сравнения
    numbers_str = list(map(str, numbers))
    # Сортировка чисел в нужном порядке
    sorted_numbers = sorted(numbers_str, key=cmp_to_key(compare), reverse=True)
    # Склеивание чисел в одно большое число
    largest_num = ''.join(sorted_numbers)
    # Убираем ведущие нули, если они есть
    largest_num = '0' if largest_num[0] == '0' else largest_num
    return int(largest_num)

# Пример использования функции
nums = [3, 30, 34, 5, 9]
print(get_biggest(nums))  # Выведет: 9534330
```
`cmp_to_key` присваивается параметру `key`, в качестве аргумента указывается функция сравнения, которая возвращает `1`, `0` или `-1`, то есть `cmp_to_key` выхватывает всевозможные пары объектов их сортируемого списка `numbers_str`, закидывает их по очереди в функцию `cpmpare(x, y)` чтобы определись, какой их объектов расположить раньше,  а какой позже. Возвращает `cmp_to_key` специальный объект-обертку, который понимает только сортировочные функции