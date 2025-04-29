#WebSocket #FastAPI 

![[Pasted image 20250428163658.png]]В этой статье мы соберем **WebSocket** маршрут, его создание не сильно отличается от создания маршрута **API** с помощью **FastAPI**. Создадим новое приложение и добавим следующий код:
```python
from fastapi.templating import Jinja2Templates
from fastapi.responses import HTMLResponse
from fastapi import FastAPI, WebSocket, Request
from starlette.websockets import WebSocketDisconnect

app = FastAPI()
templates = Jinja2Templates(directory="templates")


@app.get("/", response_class=HTMLResponse)
def read_index(request: Request):
    return templates.TemplateResponse("index.html", {"request": request})


@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    try:
        while True:
            data = await websocket.receive_text()
            await websocket.send_text(data)
    except WebSocketDisconnect as e:
        print(f'Connection closed {e.code}')
```
Самый просто способ работать с **WebSocket**, это использовать **JS**, но нам нужно будет использовать **HTML**-шаблон. Чтобы его выводить, мы создали маршрут `read_index`, который будет выводить шаблон `index.html`.

Разберем `websocket_endpoint`. Мы создали новую конечную точку `/ws`, которая открывает шлюз для нашего **WebSocket**. Приключение с **WebSocket** начинается с того, что клиент пытается установить соединение, запрашивая конечную точку `/ws`. Как только соединение установлено, сервер отвечает, принимая соединение с помощью `await.websocket.accept()`. Таким образом, наш **WebSocket** готов к отправке и приему сообщений между клиентом и сервером.

Истинная суть **WebSockets** заключается в их постоянном подключении, обеспечивающем непрерывный поток данных. Когда начинается цикл `white True`, наш **WebSocket** ожидает выходящие сообщения от клиента, используя `await websocket.receive_text()`.

Когда клиент отправляет сообщение, сервер получает его от веб-сокета в переменную `data` и отправляет ответ `await websocket.send_text(data)`

Не забываем установить зависимости:
```PowerShell
pip install websockets
pip install jinja2
```

Теперь создадим папку `templates` и внутри ее файл `index.html` следующего содержания.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FastAPI + WebSocket Example</title>
</head>
<body>
    <h1>FastAPI + WebSocket Example</h1>
    <input type="text" id="inputText" placeholder="Text...">
    <button id="submitButton">Submit</button>
    <div id="container"></div>

    <script>
        // Создаем WebSocket-соединение с сервером
        const socket = new WebSocket('ws://127.0.0.1:8000/ws');

        // Функция отображения сообщений на странице
        function showMessage(message) {
            const messageContainer = document.getElementById('container');
            const messageElement = document.createElement('div');
            messageElement.textContent = message;
            messageContainer.appendChild(messageElement);
        }

        // Обработчик события для установления соединения
        socket.addEventListener('open', (event) => {
            showMessage('Connected to server.');
        });

        // Обработчик события для получения сообщений от сервера
        socket.onmessage = (event) => {
            showMessage("You sent : " + event.data)
        }

        // Обработчик событий при закрытии соединения
        socket.addEventListener('close', (event) => {
            showMessage('Connection closed.');
        });

        const inputText = document.getElementById("inputText");
        const submitButton = document.getElementById("submitButton");

        submitButton.addEventListener("click", function() {
            const inputValue = inputText.value;
            socket.send(inputValue)
        });

    </script>
</body>
</html>
```