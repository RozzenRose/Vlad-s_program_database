#FastAPI 

### Шаг 1: Создание файлов
Создадим простой **Python** проект и добавим следующий код в `main.py`:
```python
a = 11
b = 16
print(f'The sum of {a} and {b} is {a+b}')
```
![[Pasted image 20250508133952.png]]
### Шаг 2: Создание **Dockerfile**
Создадим текстовый файл в проекте и заполним его следующим текстом:

И добавим следующий код:
```
#Выбор базового образа
FROM python:latest


#Укажем мета данные
LABEL authors="RR"


# Рабочий каталог можно выбрать любой, например, '/' или '/home' и т. д.
WORKDIR /home/rozzenrose/dock/

#Копируем удаленный файл в рабочем каталоге в контейнере
COPY main.py ./
# Теперь структура выглядит следующим образом '/usr/src/app/main.py'


#Для запуска программного обеспечения следует использовать инструкцию CMD

CMD [ "python", "./main.py"]
```
Внутри `Dockerfile` мы начнем с базового образа Python из Docker Hub. Последний тег используется для получения последнего официального образа Python.  
Очень важно установить рабочий каталог внутри контейнера. Я выбрал `/usr/src/app`. Все команды будут выполнены здесь, а образы будут скопированы только здесь.

Затем мы копируем файл `main.py` со своего компьютера в текущий рабочий каталог контейнера (`./` или `/home/rozzenrose/dock/`) с помощью команды `COPY`.
### Шаг 3: Создание .dockerignore
Создадим файл `.dockerignore` в корне нашего проекта
Теперь мы откроем нашу папку проекта. Мы видим 2 лишние папки, одна с нашей виртуальной средой, другая с настройками **Pycharm**. Чтобы они были добавлены в наш образ, мы должны добавить их имена в файл `.dockerignore`, для игнорирования их:
![[Pasted image 20250508141852.png]]
Добавим в файл `.dockerignore` следующий текст:
![[Pasted image 20250508143726.png]]