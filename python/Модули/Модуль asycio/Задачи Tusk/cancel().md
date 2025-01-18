#модуль #python #asyncio #асинхронное_программирование #Tusk 

Иногда может возникнуть необходимость отменить выполнение задачи. Это можно сделать с помощью метода `cancel()` объекта задачи. Рассмотрим пример:
```python
import asyncio

async def my_coroutine():
    try:
        print("Начало выполнения задачи")
        await asyncio.sleep(5)
        print("Задача выполнена")
    except asyncio.CancelledError:
        print("Задача была отменена")

async def main():
    task = asyncio.create_task(my_coroutine())
    
    # Начало выполнения задачи и ожидание 2 секунды
    await asyncio.sleep(2)

    # Отмена задачи
    task.cancel()
    
    await task

asyncio.run(main())
```
В этом примере:
1. **Отмена задачи**: Мы создаем задачу `my_coroutine` и отменяем ее через 2 секунды с помощью `task.cancel()`.
2. **Обработка отмены**: Внутри `my_coroutine` мы обрабатываем исключение `asyncio.CancelledError`, которое выбрасывается при отмене задачи.
