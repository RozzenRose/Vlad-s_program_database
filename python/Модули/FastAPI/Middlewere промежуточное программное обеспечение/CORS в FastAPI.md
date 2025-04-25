#FastAPI #CORS #TrustedHostMiddleware #HTTPSRedirectMiddleware #GZipMiddleware

Совместное использование ресурсов между источниками (**CORS**) служит правилом, которое предотвращает доступ незарегистрированных клиентов к ресурсу. Когда наш веб-**API** используется внешним приложением, браузер не разрешает **HTTP**-запросы из разных источников. Это означает, что доступ к ресурсам возможен только из точного источника, как **API** или источников, разрешенных **API**.

**FastAPI** предоставляет **CORS moddleware**, **CORSMiddleware**, которое позволяет нам регистрировать домены, которые могут получить доступ к нашему **API**. **Moddleware** принимает массив источников, которым будет разрешен доступ к ресурсам на сервер.

`CORSMiddleware` это класс, который реализует протокол **CORS** в качестве компонента промежуточного программного обеспечения для приложений **FastAPI**. Это позволяет настраивать различные параметры для **CORS**, такие как:
- `allow_origins`: Список источников (доменов), которым разрешен доступ к вашему API. Вы можете использовать `["*"]`, чтобы разрешить все источники, но это не рекомендуется по соображениям безопасности.
- `allow_methods`: Список методов HTTP, которые разрешены для запросов между различными источниками (cross-origin). По умолчанию он включает в себя `["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]`
- `allow_headers`: Список заголовков HTTP, которые разрешены для запросов между различными источниками. По умолчанию он включает в себя `["*"]`, что означает, что все заголовки разрешены.
- `allow_credentials`: Булевый флаг, указывающий, разрешены ли файлы cookie и заголовки авторизации для запросов между различными источниками. По умолчанию установлено значение `False`.
- `expose_headers`: Список заголовков HTTP, которые доступны браузеру в ответе. По умолчанию это пустой список.
- `max_age`: целое число, которое определяет максимальное количество секунд, в течение которых браузер может кэшировать Preflight запрос. По умолчанию он установлен на `600`.

### Как использовать CORSMiddleware
Например, чтобы разрешить доступ к нашему API только от  [https://stepik.org](https://stepik.org/), мы определяем **URL**-адреса в массиве:
```Python
origins = [
    “https://stepik.org”,
]
```
Чтобы разрешить запросы от любого клиента, массив `origins` будет содержать только одно значение - звездочку (`*`). Звездочка - это подстановочный знак, который указывает нашему **API** разрешать запросы  из любого места.

Чтобы использовать **CORSMiddleware** в приложении **FastAPI**, вам нужно импортировать его из `fastapi.middleware.cors` и добавить в экземпляр приложения с помощью метода `add_meddleware`. Например:
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:3000",
    "https://example.com",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


@app.get("/")
async def main():
    return {"message": "Hello World"}
```
На этот раз мы использовали функцию `add_middleware()` **FastAPI**, чтобы добавить поддержку **CORS** в наше приложение. Помимо `allow_origins`, нам также необходимо добавить в **CORSMiddleware** параметр `allow_credentials`, который добавляет `Access-Control_Allow_Credentials: true` в заголовок ответа, чтобы браузер мог распознавать совпадения адресов домена и отправлять файл `cookie` авторизации для разрешения запроса.

Чтобы проверить, работает ли наша конфигурация **CORS**, как ожидалось, отправим запрос на **API**. Для этого создадим любой **html** файл, следующего содержания:
```html
<!DOCTYPE html>
<html>
   <body>
      <h1>Send JS request</h1>
      <button onclick="getData()">Click me</button>
      <script>
         function getData() {
            fetch('http://127.0.0.1:8000/')
               .then(resp => resp.json())
               .then(data => {
                  console.log(data);
               })
               .catch(error => {
                  console.error(error);
               });
         }
      </script>
   </body>
</html>
```
Запустим **FastAPI** через **uvicorn** сервер, и откроем **html** файл в браузере. В нем нажмем кнопку **Click me** и посмотрим логи бразуера:
![[Pasted image 20250422140355.png]]
Мы получим ошибку, так как нашему приложению разрешены запросы лишь от следующих адресов:
```
    "http://localhost:3000",
    "https://example.com",
```
Если браузер делает запрос из локального файла (например, `file://.../main.html`), мы сталкиваемся с тем, что наш источник запроса должен быть указан как `null`. Потому что **HTML** файл является источником запроса, а у него адрес `null`. Изменим этот список разрешенных источников:
```python
origins = [
    "http://localhost:3000",
    "https://example.com",
    "null"
]
```
И теперь открыв **HTML** файл в браузере и сделав запрос, мы увидим в консоли следующий текст:
```css
{message: "Hello World"}
```
![[Pasted image 20250422140901.png]]
Вы также можете проверить заголовки ответов и посмотреть их в браузере:
```http
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: null
Content-Length: 25
Content-Type: application/json
Date: Thu, 02 May 2024 06:50:07 GMT
Server: uvicorn
Vary: Origin
```
**CORS** - это полезная функция для веб-приложений, которым необходимо взаимодействовать с **API**, размещенными на разных серверах или доменах. **FastAPI** позволяет легко включать и настраивать **CORS** для ваших точек **API** всего за несколько строк кода.
### TrustedHostMiddleware
Проверять заголовок хоста входящих запросов для предотвращения потенциальных атак заголовка хоста **HTTP**.
```python
from fastapi import FastAPI
from fastapi.middleware.trustedhost import TrustedHostMiddleware

app = FastAPI()

app.add_middleware(
    TrustedHostMiddleware, allowed_hosts=["example.com", "*.example.com"]
)


@app.get("/")
async def main():
    return {"message": "Hello World"}
```
- `allowed_hosts`- Список доменных имен, которые должны быть разрешены в качестве имен хостов. Также синтаксис поддерживает поддомены -  `*.example.com`. Чтобы разрешить любое имя хоста можно использовать `allowed_hosts=["*"]`.
Если входящий запрос не будет проверен правильно, будет отправлен код ошибка `400`.
### HTTPSRedirectMiddleware
Обеспечивает, чтобы все входящие запросы были `https`.  Любые запросы к `http` будут перенаправлены на `https`
```python
from fastapi import FastAPI
from fastapi.middleware.httpsredirect import HTTPSRedirectMiddleware

app = FastAPI()

app.add_middleware(HTTPSRedirectMiddleware)


@app.get("/")
async def main():
    return {"message": "Hello World"}
```
### GZip Middleware
**GZip** - это алгоритм сжатия. Используется очень часто для сжатия контента, передаваемого через Интернет, и учитывая, что он поддерживается браузерами, то он способен сильно уменьшить размер файлов **JavaScript**, **CSS**, **HTML**
```python
from fastapi import FastAPI
from fastapi.middleware.gzip import GZipMiddleware

app = FastAPI()

app.add_middleware(GZipMiddleware, minimum_size=1000)


@app.get("/")
async def main():
    return "somebigcontent"
```
- `minimum_size`- Не делать сжатие ответов, которые меньше этого минимального размера в байтах. По умолчанию `500`.