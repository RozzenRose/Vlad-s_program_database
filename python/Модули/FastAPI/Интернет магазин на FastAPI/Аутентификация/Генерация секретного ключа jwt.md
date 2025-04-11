#FaseAPI #jwt

Прежде чем мы приступим к построению схемы аутентификации, нам сначала необходимо сгенерировать секретный ключ, который является важным элементом создания подписи. JWT имеет заголовок подписи и шифрования объектов JSON (JOSE), который представляет собой метаданные, описывающие, какой алгоритм использовать для кодирования обычного текста, а полезная нагрузка — это данные, которые нам нужно закодировать в токен. Когда клиент запрашивает вход в систему, сервер авторизации подписывает JWT с помощью секретного ключа, который будет сгенерирован с помощью алгоритма. Этот секретный ключ представляет собой закодированную строку, созданную вручную, и ее следует хранить отдельно на сервере авторизации.

Вот так выглядит общая схема авторизации через **JWT**:
![[JWT.pdf]]

`openssl` - подходящая утилита для генерации этого длинного и рандомного ключа. Запустим следующую команду, чтобы создать ключ:
```cmd
openssl rand -hex 32
```
Мы получим следующий результат:
![[Pasted image 20250411133733.png]]
В нашем проекте мы будем хранить секретный ключ и тип алгоритма в переменных. Для этого в файл `routers/auth.py` добавим следующий код:
```python
SECRET_KEY = '73e234e9792c5700bbb8d9de25e54bd2004bd6e490fab7e1bfa8ec2bf181fb1e'
ALGORITHM = 'HS256'
```
В рамках расширения JWT Python мы выбрали модуль [`PyJWT`](https://pypi.org/project/PyJWT/) для генерации токена, поскольку он надежен и имеет дополнительные криптографические функции, которые могут подписывать сложное содержимое данных. Прежде чем использовать этот модуль, сначала установим его с помощью команды pip.
```Power Shell
pip install PyJWT
```
Продолжим работу с файлом `routers/auth.py` и добавим следующую функцию:
```python
from datetime import datetime, timedelta, timezone
import jwt


async def create_access_token(username: str, user_id: int, is_admin: bool, is_supplier: bool, is_customer: bool,
                              expires_delta: timedelta):
    payload = {
        'sub': username,
        'id': user_id,
        'is_admin': is_admin,
        'is_supplier': is_supplier,
        'is_customer': is_customer,
        'exp': datetime.now(timezone.utc) + expires_delta
    }

    # Преобразование datetime в timestamp (количество секунд с начала эпохи)
    payload['exp'] = int(payload['exp'].timestamp())
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)
```
Итак, конечная точка авторизации будет вызывать метод `create_access_token()` для запроса JWT. Служба входа предоставляет данные, обычно имя пользователя, которые составляют полезную часть токена. Поскольку JWT должен быть кратковременным, процесс должен обновить часть полезной нагрузки с истекшим сроком действия до некоторого значения даты и времени в минутах или секундах, подходящих для приложения.

Реализация службы `login` аналогична предыдущей аутентификации OAuth2 на основе пароля, за исключением того, что в этой версии есть вызов `create_access_token()` для генерации JWT.
```python
@router.post('/token')
async def login(db: Annotated[AsyncSession, Depends(get_db)], form_data: Annotated[OAuth2PasswordRequestForm, Depends()]):
    user = await authenticate_user(db, form_data.username, form_data.password)

    token = await create_access_token(user.username, user.id, user.is_admin, user.is_supplier, user.is_customer,
                                expires_delta=timedelta(minutes=20))
    return {
        'access_token': token,
        'token_type': 'bearer'
    }
```

Конечная точка по-прежнему должна возвращать `access_token` и `token_type`, поскольку это по-прежнему аутентификация OAuth2 на основе пароля, которая извлекает учетные данные пользователя из `OAuth2PasswordRequestForm`.

Запустим сервер и проверим работу. Теперь если мы попробуем авторизоваться через конечную точку, мы получим токен для работы.
![[Pasted image 20250411140959.png]]

В отличие от предыдущей схемы OAuth2, мы будем внедрять зависимость `get_current_user()` в каждую службу API, чтобы обеспечить безопасность и ограничить доступ. Внедренный экземпляр `OAuth2PasswordBearer` вернет JWT для извлечения полезных данных с использованием декодеров с указанным алгоритмом декодирования. Если токен подделан, изменен или срок его действия истек, метод вызовет исключение.

Напишем эту функцию в файле `routers/auth.py`:
```python
async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str | None = payload.get('sub')
        user_id: int | None = payload.get('id')
        is_admin: bool | None = payload.get('is_admin')
        is_supplier: bool | None = payload.get('is_supplier')
        is_customer: bool | None = payload.get('is_customer')
        expire: int | None = payload.get('exp')

        if username is None or user_id is None:
            raise HTTPException(
                status_code=status.HTTP_401_UNAUTHORIZED,
                detail='Could not validate user'
            )
        if expire is None:
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail="No access token supplied"
            )

        if not isinstance(expire, int):
            raise HTTPException(
                status_code=status.HTTP_400_BAD_REQUEST,
                detail="Invalid token format"
            )

        # Проверка срока действия токена
        current_time = datetime.now(timezone.utc).timestamp()

        if expire < current_time:
            raise HTTPException(
                status_code=status.HTTP_401_UNAUTHORIZED,
                detail="Token expired!"
            )

        return {
            'username': username,
            'id': user_id,
            'is_admin': is_admin,
            'is_supplier': is_supplier,
            'is_customer': is_customer,
        }
    except jwt.ExpiredSignatureError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Token expired!"
        )
    except jwt.exceptions:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail='Could not validate user'
        )
```
В этом коде, мы декодируем полученный токен в словарь `payload`, используя секретный ключ и алгоритм. Далее мы извлекаем содержимое ключей словаря, и запускаем проверки. Сначала функция проверяет ключи username и user_id. Если содержимое данных ключей не найдено, то вызываем ошибку валидации пользователя. Далее функция проверяет срок действия токена. Если срок действия не указан, значит, токен не был предоставлен. Следующая проверка — валидность токена — генерируется исключение, информирующее пользователя об истечении срока действия токена. Если токен действителен, возвращается декодированная полезная нагрузка.

В блоке except для любой ошибки JWT выдается исключение неверного запроса.

Теперь, когда мы реализовали функции для создания и проверки токенов, отправляемых в приложение, давайте создадим функцию, которая проверяет аутентификацию пользователя и служит зависимостью.

`get_current_user()` должен быть внедрен в каждую реализацию сервиса, чтобы ограничить доступ пользователей. Но на этот раз метод не только проверит учетные данные, но и выполнит декодирование полезной нагрузки JWT.

Изменим зависимости эндпоинта `read_current_user:`
```python
@router.get('/read_current_user')
async def read_current_user(user: dict = Depends(get_current_user)):
    return {'User': user}
```

Запустим сервер и проверим работу. Нажмем на кнопку «`Authorize`» в правом верхнем углу документации и заполним форму входа. Далее перейдем к эндпоинту `read_current_user` и попробуем отправить запрос.
![[Pasted image 20250411143425.png]]В заголовке запроса у нас присутствует сам код токена. И в теле ответа увидим все полученные данные из токена. В случае если срок действия токена закончился, мы увидим сообщение об ошибке

