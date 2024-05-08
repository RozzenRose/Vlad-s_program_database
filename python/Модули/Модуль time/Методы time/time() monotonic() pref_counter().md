#метод #python модуль #time #monotonic #pref_counter

### time() time_ns() секунды с начала эпохи
Возвращает количество секунд, прошедших с начла эпохи. `time()` вернет `float`  с секундами и миллисекундами. `time_ns()` вернет `int()` с микросекундами
```python
import time

seconds = time.time()
print('Количнство секунд с начала эпохи = ', seconds)
```
```
Количество секунд с начала эпохи = 1630387918.354396
```
Эпоха Unix началась с 1 января 1970 года.
Позволяет удобно находить время выполнение программы
```python
import time

start_time = time.time()

for i in range(5): 
    print(i)
    time.sleep(1)

end_time = time.time()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```
```
0
1
2
3
4
Время работы программы = 5.022884845733643
```
### monotonic() monotonic_ns() измерение времени
Работает так же как методы `time()` и `time_ns()`, но `monotonic()` никогда не вернет, значения ниже, ниже значения, полученного при предыдущем вызове
`monotonic()` вернет `float`, `monotonic_ns()` вернет `int`
```python
import time

start_time = time.monotonic()

for i in range(5): 
    print(i)
    time.sleep(0.5)

end_time = time.monotonic()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```
```
0
1
2
3
4
Время работы программы = 2.547000000020489
```

### pref_counter() очень точное измерение времени
Работает точно так же как методы `time()` или `monitoric()` но использует в работе таймер с самым высоким разрешением.
```python
import time

start_time = time.perf_counter()

for i in range(5): 
    print(i)
    time.sleep(1)

end_time = time.perf_counter()

elapsed_time = end_time - start_time
print(f'Время работы программы = {elapsed_time}')
```
```
0
1
2
3
4
Время работы программы = 5.042140900040977
```