#docjer #FastAPI 

![[Pasted image 20250624164926.png]]
За основу возьмем простой проект по типу **Hello world**. Для этого создадим файл `main.py` следующего содержания:
```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def welcome() -> dict:
    return {"message": "Hello World"}
```
И следующим шагом выполним команду:
```
pip freeze > requirements.txt
```
Чтобы создать файл, содержащий все зависимости нашего проекта.

### Создание Dockerfile
Начнем с `Dockerfile` - это текстовый файл, который содержит инструкцию о том, как будет создан `docker`-образ.

В `Dockerfile` используются нижеследующие директивы:
- `FROM`: Директива устанавливает базовый образ, из которого будет построен контейнер **Docker**
- `WORKDIR`: директива устанавливает рабочий каталог в созданном образе
- `RUN`: Директива выполняет команды в контейнере.
- `COPY`: Директива копирует файлы из файловой системы в контейнер
- `CMD`: Директива устанавливает исполняемые команды в контейнере

В корневом каталоге проекта создайте файл с именем `Dockerfile` без расширения файла

Добавим в этот файл следующие команды:
```dockerfile
# pull the official base image
FROM python:3.12.0-alpine

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# set work directory
WORKDIR /usr/src/app

# copy requirements.txt file to work directory
COPY requirements.txt .

# update pip and install dependencies
RUN pip install --upgrade pip && pip install -r requirements.txt

# copy project to work directory
COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
```

Разберем содержимое этого файла подробнее:
- `FROM python:3.12.0-alpine`: Устанавливает базовый образ, на основе которого будет создан контейнер **Docker**
- `ENV PYTHONDONTWRITEBYTECODE=1`: Не позволяет **Python** создавать файлы с `.pyc` в контейнере
- `ENV PYTHONUNDUFFERED=1`: Гарантирует, что вывод **Python** регистрируется в терминале, что позволяет отслеживать логи **FastAPI** в режиме реального времени.
- `WORKDIR /usr/src/app`: Устанавливает рабочий каталог внутри контейнера в `/usr/src/app`
- `COPY requirements.txt .`:Копирует файл зависимостей `requirements.txt` в рабочий каталог в контейнере.
- `RUN pip imstall --upgrade pip && pip install -r requirements.txt`: Устанавливает и обновляет версию `pip`, которая находится в контейнере, а затем устанавливает все необходимые модули проекта для запуска в контейнере.
- `COPY . .`: Копирует весь исходный код проекта в рабочий каталог в контейнере.
- `CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]`: Устанавливает исполняемые команды в контейнере

### Создание образа **Docker**
Перед началом создания образа, создадим файл `.dockerignore` следующего содержания:
```.dockerignore
.venv
__pycache__
.idea
*/Dockerfile*
.dockerignore
```
И теперь мы можем приступить к созданию `Docker image` из `Dockerfile`, который мы создали выше, выполните следующую команду:
```
docker build --tag fastapi_hello_world:latest
```
- `--tag` Устанавливает тег для образа. Например, мы создаем образ `Docker` из `pythob:3.12.0` у него есть тег `alpine`. В нашем образе `Docker`, `latest` это тег.
- Точка `.` указывает на то, что `Dockerfile` находится в текущем рабочем каталоге.
![[Pasted image 20250625153016.png]]Чтобы перечислить все доступные образы на вашем компьютере, выполните следующую команду:
```
docker image ls
```
![[Pasted image 20250625153401.png]]
### Создание и запуск контейнера **Docker**
Чтобы создать и запустить контейнер **Docker** из образа, который мы создали выше, выполним команду:
```bash
docker run --name fastapi_hello_word -d -p 80:80 fastapi_hello_world:latest
```
- `--name`: Устанавливает имя контейнера **Docker**
- `-d`: Заставляет образ работать в фоновом режиме
- `-p 80:80`: Сопоставляет порт `80` в контейнере **Docker** с портом `80` на локальном хосте.
- `fastapi_hello_world:latest`: Указывает, какой образ используется для сборки контейнера **Docker**

После выполнения команды, мы получим следующий ответ:
![[Pasted image 20250625161137.png]]Посмотрим на работу **API** приложения через браузер, откроем http://127.0.0.1/:
![[Pasted image 20250625161414.png]]
Как мы видим, наше приложение прекрасно работает из **Docker** - контейнера.

Но это приложение не взаимодействует с внешними ресурсами. Такие приложения встречаются достаточно редко. Большинство реальных проектов представляют собой совокупность взаимодействующих приложений.