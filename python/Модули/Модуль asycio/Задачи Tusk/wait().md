#модуль #python #asyncio #асинхронное_программирование #Tusk 

Функция `asyncio.wait` предоставляет более гибкий способ управления задачами. Она позволяет выбирать между ожиданием завершения всех задач или завершением любой из них, а также позволяет устанавливать таймауты. Она также принимает в качестве аргументов как сопрограммы, так и задачи, оборачивая сопрограммы в задачи под капотом.
### Пример использования `asyncio.wait`
Рассмотрим пример, где мы используем `asyncio.wait` для ожидания завершения всех задач:
```python
import asyncio


async def task1():
    await asyncio.sleep(1)
    print("Задача 1 завершена")
    return "Результат 1"


async def task2():
    await asyncio.sleep(2)
    print("Задача 2 завершена")
    return "Результат 2"


async def main():
    tasks = [asyncio.create_task(task1()), asyncio.create_task(task2())]
    
    done, pending = await asyncio.wait(tasks, return_when=asyncio.ALL_COMPLETED)
    
    for task in done:
        print(f"Результат: {task.result()}")


asyncio.run(main())
```
```
Задача 1 завершена
Задача 2 завершена
Результат: Результат 2
Результат: Результат 1
```
В этом примере:
- **Создание задач**: Мы создаем две задачи `task1` и `task2` с использованием `asyncio.create_task`.
- **Ожидание завершения задач**: `asyncio.wait(tasks, return_when=asyncio.ALL_COMPLETED)` ожидает завершения всех задач из списка `tasks`. Параметр `return_when=asyncio.ALL_COMPLETED` является значением по умолчанию и не обязателен для передачи.
- **Обработка результатов**: После завершения задач результаты выводятся на экран.
### Использование `asyncio.wait` для завершения первой задачи
Иногда необходимо дождаться завершения только первой задачи, а не всех. Это можно сделать, изменив параметр `return_when` на `asyncio.FIRST_COMPLETED`.
```python
import asyncio


async def task1():
    await asyncio.sleep(1)
    print("Задача 1 завершена")
    return "Результат 1"


async def task2():
    await asyncio.sleep(5)
    print("Задача 2 завершена")
    return "Результат 2"


async def main():
    tasks = [asyncio.create_task(task1()), asyncio.create_task(task2())]
    
    done, pending = await asyncio.wait(tasks, return_when=asyncio.FIRST_COMPLETED)
    
    for task in done:
        print(f"Результат: {task.result()}")
    
    for task in pending:
        print(f"Задача не завершена: {task}")


asyncio.run(main())
```
```
Задача 1 завершена
Результат: Результат 1
Задача не завершена: <Task pending name='Task-3' coro=<task2() running at F:\ACC_helper\test.py:11> wait_for=<Future pending cb=[Task.task_wakeup()]>>

```
В этом примере:
- **Ожидание первой завершенной задачи**: `asyncio.wait(tasks, return_when=asyncio.FIRST_COMPLETED)` ожидает завершения первой задачи из списка `tasks`.
- **Обработка результатов**: Завершенные и незавершенные задачи обрабатываются отдельно.
### Использование таймаутов с `asyncio.wait`
Иногда важно установить ограничение по времени для выполнения задач. Это можно сделать с помощью параметра `timeout` в функции `asyncio.wait`.
```python
import asyncio


async def task1():
    await asyncio.sleep(1)
    print("Задача 1 завершена")
    return "Результат 1"


async def task2():
    await asyncio.sleep(5)
    print("Задача 2 завершена")
    return "Результат 2"


async def main():
    tasks = [asyncio.create_task(task1()), asyncio.create_task(task2())]
    
    done, pending = await asyncio.wait(tasks, timeout=3)
    
    for task in done:
        print(f"Результат: {task.result()}")
    
    for task in pending:
        print(f"Задача не завершена: {task}")


asyncio.run(main())
```
```
Задача 1 завершена
Результат: Результат 1
Задача не завершена: <Task pending name='Task-3' coro=<task2() running at F:\ACC_helper\test.py:11> wait_for=<Future pending cb=[Task.task_wakeup()]>>
```
В этом примере:
- **Таймаут для ожидания задач**: `asyncio.wait(tasks, timeout=3)` ожидает завершения задач с таймаутом в 3 секунды.
- **Обработка незавершенных задач**: Задачи, не завершившиеся в течение таймаута, остаются в наборе `pending`.