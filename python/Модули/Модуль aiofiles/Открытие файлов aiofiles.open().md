#модуль #python #aiofiles #асинхронное_программирование

Файлы в `aiofiles` открываются с использованием контекстного менеджера `async with`. Это позволяет гарантировать, что файл будет закрыт правильно после завершения работы с ним
```python
import aiofiles

async with aiofiles.open('example.txt', mode='r') as f:
    # Операции с файлом
```