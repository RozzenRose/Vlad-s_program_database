#python #pip 
### Установка зависимостей
```bush
pip install lib.name
```
### Перенос зависимостей
Для того что бы перечислить все зависимости проекта в файл `requirements.txt`:
```bash
pip freeze > requirements.txt
```

Для того что бы установить все зависимости файла в проект:
```bash
pip install -r requirements.txt
```
### Виртуальное  окружение
Создание виртуального окружения:
```bash
python -m venv .venv
```

Активация виртуального окружения:

| Операционная система     | Команда активации            |
| ------------------------ | ---------------------------- |
| **Windows (cmd)**        | `.venv\Scripts\activate.bat` |
| **Windows (PowerShell)** | `.venv\Scripts\Activate.ps1` |
| **macOS / Linux / WSL**  | `source .venv/bin/activate`  |
