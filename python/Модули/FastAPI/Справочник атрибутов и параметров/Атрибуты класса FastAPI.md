#FastAPI 

![[Pasted image 20250327180724.png]]
```python
from fastapi import FastAPI

app = FastAPI()
```
---
Атрибуты класса `FastAPI()`:
```python
FastAPI(
    *,
    debug=False,
    routes=None,
    title="FastAPI",
    summary=None,
    description="",
    version="0.1.0",
    openapi_url="/openapi.json",
    openapi_tags=None,
    servers=None,
    dependencies=None,
    default_response_class=Default(JSONResponse),
    redirect_slashes=True,
    docs_url="/docs",
    redoc_url="/redoc",
    swagger_ui_oauth2_redirect_url="/docs/oauth2-redirect",
    swagger_ui_init_oauth=None,
    middleware=None,
    exception_handlers=None,
    on_startup=None,
    on_shutdown=None,
    lifespan=None,
    terms_of_service=None,
    contact=None,
    license_info=None,
    openapi_prefix="",
    root_path="",
    root_path_in_servers=True,
    responses=None,
    callbacks=None,
    webhooks=None,
    deprecated=None,
    include_in_schema=True,
    swagger_ui_parameters=None,
    generate_unique_id_function=Default(generate_unique_id),
    separate_input_output_schemas=True,
    **extra
)
```
---
Разберем подробнее большинство из них: 

|**Параметр**|**Описание**|
|---|---|
|`debug`|Логическое значение, указывающее, следует ли возвращать отладочные трассировки при ошибках сервера.<br><br>Посмотреть [документацию Starlette для приложений](https://www.starlette.io/applications/#instantiating-the-application).<br><br>**Тип:** `bool`**:** `False`|
|`routes`|Обычно вы не используете этот параметр с FastAPI, он унаследован от Starlette и поддерживается для совместимости. Этот параметр принимает список маршрутов для обслуживания входящих запросов HTTP и WebSocket.<br><br>**Тип:** `Optional[List[BaseRoute]]`**По умолчанию:** `None`|
|`title`|Название вашего API. Оно будет добавлено в OpenAPI (например, в документации `/docs`).<br><br>**Пример:**<br><br>```java<br>from fastapi import FastAPI<br><br>app = FastAPI(title="My_First_Project")<br>```<br><br>**Тип:** `str`**:** `'FastAPI'`|
|`summary`|Краткое описание API. Оно будет добавлено в OpenAPI (например, в документации `/docs`).<br><br>**Пример**<br><br>```java<br>from fastapi import FastAPI<br><br>app = FastAPI(summary="My CRUD application.")<br>```<br><br>**Тип:** `Optional[str]`**По умолчанию:** `None`|
|`description`|Описание API. Поддерживает Markdown (с использованием [синтаксиса CommonMark](https://commonmark.org/)). Оно будет добавлено в OpenAPI (например, в документации `/docs`).<br><br>**Пример** <br><br>```python<br>from fastapi import FastAPI<br>app = FastAPI( description="""The CRUD application supports writing,<br>                              reading, updating and deleting posts.""")<br>```<br><br>**Тип:** `str`**:** `''`|
|`version`|Версия API. Она будет добавлена в OpenAPI (например, в документации `/docs`).<br><br>**Обратите внимание**, что это версия вашего приложения, а не версия спецификации OpenAPI и не используемая версия FastAPI.<br><br>**Пример**<br><br>```java<br>from fastapi import FastAPI<br><br>app = FastAPI(version="0.0.1")<br>```<br><br>**Тип:** `str`**:** `'0.1.0'`|
|`openapi_url`|URL-адрес, на котором будет доступна схема OpenAPI.<br><br>**Пример**<br><br>```java<br>from fastapi import FastAPI<br><br>app = FastAPI(openapi_url="/api/v1/openapi.json")<br>```<br><br>**Тип:** `Optional[str]`  **По умолчанию:** `'/openapi.json'`|
|`openapi_tags`|Список тегов, используемых OpenAPI, это те же теги, которые вы можете установить в операциях пути, например:<br><br>- `@app.get("/users/", tags=["users"])`<br>- `@app.get("/items/", tags=["items"])`<br><br>Теги можно использовать для указания порядка, показанного в таких документациях, как пользовательский интерфейс Swagger, используемых в `/docs`.<br><br>Узнайте больше в [документации FastAPI для URL-адресов метаданных](https://fastapi.tiangolo.com/tutorial/metadata/#metadata-for-tags).<br><br>**Тип:** `Optional[List[Dict[str, Any]]]` **По умолчанию:** `None`|
|`servers`|Список словарей с информацией о подключении к целевому серверу.<br><br>Вы можете использовать это, например, если ваше приложение обслуживается из разных доменов, и вы хотите использовать один и тот же пользовательский интерфейс Swagger для взаимодействия с каждым из них (вместо того, чтобы открывать несколько вкладок браузера).<br><br>Узнайте больше в [документации FastAPI](https://fastapi.tiangolo.com/advanced/behind-a-proxy/#additional-servers).<br><br>**Пример**<br><br>```ini<br>app = FastAPI(servers=[{"url": "https://stag.example.com", "description": "Staging"},<br>                       {"url": "https://prod.example.com", "description": "Production"}, ])<br>```<br><br>**Тип:** `Optional[List[Dict[str, Union[str, Any]]]]`**По умолчанию:** `None`|
|`default_response_class`|Используемый класс ответа. Узнайте больше в [документации FastAPI для пользовательского ответа - HTML, Stream, File и другие](https://fastapi.tiangolo.com/advanced/custom-response/#default-response-class).<br><br>**Пример** <br><br>```javascript<br>from fastapi import FastAPI<br>from fastapi.responses import ORJSONResponse<br><br>app = FastAPI(default_response_class=ORJSONResponse)<br>```<br><br>**Тип:** `Type[Response]` **По умолчанию:** `Default(JSONResponse)`|
|`redirect_slashes`|Следует ли обнаруживать и перенаправлять косые черты в URL-адресах, когда клиент не использует тот же формат.<br><br>**Пример**  <br><br>```python<br>from fastapi import FastAPI<br>app = FastAPI(redirect_slashes=True)  # the default<br><br><br>@app.get("/items/")<br>async def read_items():<br>    return {"item_id": "Foo"}<br>```<br><br>С помощью этого параметра, если клиент переходит в `/items` (без завершения косой черты), он будет автоматически перенаправлен с кодом состояния HTTP 307 на `/items/`.<br><br>**Тип:** `bool`**:** `True`|
|`docs_url`|Путь к автоматической интерактивной документации API. URL-адрес по умолчанию - `/docs`. Вы можете отключить его, установив в `None`. Мы это рассматривали в разделе [1.4](https://stepik.org/lesson/1205951/step/3?unit=1219159) данного курса.<br><br>**Тип:** `Optional[str]` **По умолчанию:** `'/docs'`|
|`redoc_url`|Путь к альтернативной автоматической интерактивной документации API, предоставленной ReDoc.<br><br>URL-адрес по умолчанию - `/redoc`. Вы можете отключить его, установив в `None`. Мы это рассматривали в разделе [1.4](https://stepik.org/lesson/1205951/step/3?unit=1219159) данного курса.<br><br>**Тип:** `Optional[str]` **По умолчанию:** `'/redoc'`|
|`swagger_ui_oauth2_redirect_url`|Конечная точка перенаправления OAuth2 для пользовательского интерфейса Swagger.<br><br>По умолчанию это `/docs/oauth2-redirect`.<br><br>Это используется только в том случае, если вы используете OAuth2 (с кнопкой "Авторизовать") с пользовательским интерфейсом Swagger.<br><br>**Тип:** `Optional[str]` **По умолчанию:** `'/docs/oauth2-redirect'`|
|`swagger_ui_init_oauth`|Конфигурация OAuth2 для пользовательского интерфейса Swagger, по умолчанию отображается в `/docs`.<br><br>**Тип:** `Optional[Dict[str, Any]]`  **По умолчанию:** `None`|
|`middleware`|Список промежуточного программного обеспечения, которое будет добавлено при создании приложения.<br><br>В FastAPI вы обычно делаете это с помощью `app.add_middleware()`.<br><br>**Тип:** `Optional[Sequence[Middleware]]` **По умолчанию:** `None`|
|`exception_handlers`|Словарь с обработчиками исключений. В FastAPI вы обычно используете декоратор `@app.exception_handler()`.<br><br>**Тип:**`Optional[Dict[Union[int, Type[Exception]], Callable[[Request, Any], Coroutine[Any, Any, Response]]]]` **По умолчанию:** `None`|
|`contact`|Контактная информация для открытого API. Может содержать несколько полей:<br><br>\|   \|   \|<br>\|---\|---\|<br>\|`name`\|Идентификационное имя контактного лица/организации.\|<br>\|`url`\|URL указывающий на контактную информацию.\|<br>\|`email`\|Email адрес контактного лица/организации.\|<br><br>**Тип:**`Dict` **По умолчанию:** `None`|
|`license_info`|Информация о лицензии открытого API. Может содержать несколько полей:<br><br>\|   \|   \|<br>\|---\|---\|<br>\|`name`\|**Обязательный,** если установлен параметр `license_info`. Название лицензии, используемой для API\|<br>\|`url`\|URL, указывающий на лицензию, используемую для API.\|<br><br>**Тип:**`Dict` **По умолчанию:** `None`|
|`terms_of_service`|Ссылка к условиям пользования API.<br><br>**Тип:**`Str` **По умолчанию:** `None`|
|`root_path`|Префикс пути, обрабатываемый прокси, который не виден приложению, но виден внешними клиентами, что влияет на такие вещи, как пользовательский интерфейс Swagger.<br><br>**Пример**<br><br>```java<br>from fastapi import FastAPI<br><br>app = FastAPI(root_path="/api/v1")<br>```<br><br>**Тип:** `str`**:** `''`|
