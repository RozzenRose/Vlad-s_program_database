#python #treading #многопоточность 

Модуль позволяющий запускать разные функции и методы в несколько потоков.
```python
import threading
import time

def count_numbers():
    for i in range(1, 6):
        print(f"[Числа] {i}")
        time.sleep(1)

def print_letters():
    for letter in ['A', 'B', 'C', 'D', 'E']:
        print(f"[Буквы] {letter}")
        time.sleep(1)

# Создаём два потока
thread1 = threading.Thread(target=count_numbers)
thread2 = threading.Thread(target=print_letters)

# Запускаем потоки
thread1.start()
thread2.start()

# Ждём завершения обоих
thread1.join()
thread2.join()

print("✅ Оба потока завершены")
```

Эта строка создает поток на основе метода `count_numbers()`, этот поток останется хранится в переменной `thread1`:
```python
thread1 = threading.Thread(target=count_numbers)
```
Если применить к потоку метод `start()`, поток начнет выполнятся
А при применении метода `.join()` блокируется поток `main`, он будет разблокирован только только когда поток будет выполнен.

В примере выше, мы запускаем два потока, и блокируем `main` на время выполнения этих потоков.

#### Запуск потоков с аргументами
Иногда нам нужно запустить поток на основе какого-нибудь метода с применением аргументов:
```python
def greet(name):
    print(f"Привет, {name}")

t = threading.Thread(target=greet, args=("RozzenRose",))
t.start()
```

#### Запуск и остановка пулов потоков
Иногда нужно запускать потоки некоторыми наборами. Мы можем хранить объекты потоков в разных итерируемых объектах, а потом запускать их все разом
```python
threads = [threading.Thread(target=add_items) for _ in range(10)]
[t.start() for t in threads]
[t.join() for t in threads]
```
Мы создаем `list` из 10ти потоков на основе некой функции `add_items()`. Затем запускам их всех генераторным выражением и ожидаем окончаниях этих потоков.