#python #pytest #тесты 

У нас есть два эндпоинта, один из которых создает запись о доходе, а второй получает все текущие записи о доходах:
```python
@router.post('/new_income', status_code=status.HTTP_201_CREATED)
async def create_income(db: Annotated[AsyncSession, Depends(get_db)],
                        income: CreateIncome,
                        user: Annotated[dict, Depends(get_current_user)]):
    await create_income_in_db(db, user.get('user_id'), income.description, income.quantity,
                              income.currency)
    return {'status_code': status.HTTP_201_CREATED,
            'transaction': 'Income created successfully'}


@router.get('/incomes_current_month')
async def get_incomes_current_month(db: Annotated[AsyncSession, Depends(get_db)],
                                    user: Annotated[dict, Depends(get_current_user)],
                                    current: str | None = None):
    if current not in ('EUR', 'RUB', 'RSD', None):
        raise HTTPException(status_code=422, detail="Currency error: choose only EUR/RUB/RSD or leave this field blank")
    future = asyncio.get_running_loop().create_future()
    raw_data = await get_incomes_current_from_db(db, user.get('user_id'))
    reply_queue, consumer_tag = await rpc_incomes_request(future, raw_data, current)

    try:
        response = json.loads(await asyncio.wait_for(future, timeout=10))
        return {'incomes': raw_data} | response
    except asyncio.TimeoutError:
        return {'incomes': raw_data,
                'euro': "CurrentAggregator doesn't response",
                'rub': "CurrentAggregator doesn't response",
                'rsd': "CurrentAggregator doesn't response",
                'answer': "CurrentAggregator doesn't response"}
    finally:
        await reply_queue.cancel(consumer_tag)

```
Напишем для них тесты:
```python
import pytest
from fastapi.testclient import TestClient
from unittest.mock import AsyncMock, ANY
from app.database.db_depends import get_db
from app.functions.auth_functions import get_current_user
from app.main import app


client = TestClient(app) # достаем приложение FastAPI из main и создаем 
						 # на его основе тестовый клиент

@pytest.fixture # фикстуры - это мок функции, эта возвращает мок сессии
def override_get_db():
    async def fake_db():
        yield AsyncMock()  # фейковая сессия SQLAlchemy
    return fake_db


@pytest.fixture # Возвращает тестовые данные пользователя
def override_get_current_user():
    def fake_user():
        return {'user_id': 123,
                'username': 'test_username',
                'email': 'test@gmail.com',
                'is_admin': False,
                'expire': False}
    return fake_user


@pytest.fixture # заменяем реальные функции фоковыми
def client_with_overrides(override_get_db, override_get_current_user):
    app.dependency_overrides[get_db] = override_get_db
    app.dependency_overrides[get_current_user] = override_get_current_user
    with TestClient(app) as c:
        yield c
    app.dependency_overrides = {}  # сбросить после теста


@pytest.mark.asyncio
async def test_create_income(monkeypatch, client_with_overrides):
    """Test successful creation of a new income record."""
    # Мокаем саму функцию записи в базу
    mock_create_income_in_db = AsyncMock()
    monkeypatch.setattr("app.routers.incomes.create_income_in_db", mock_create_income_in_db)

    payload = { # Собираем JSON для отправки в ендпоинт
              "description": "Salary",
              "quantity": 1000,
              "currency": "EUR"
    }

    # Отправляем запрос в ендпоинт
    response = client_with_overrides.post("/incomes/new_income", json=payload)

    # Ожидаемые события
    assert response.status_code == 201
    data = response.json()
    assert data["transaction"] == "Income created successfully"

    # Проверяем, что create_income_in_db была вызвана с правильными аргументами
    mock_create_income_in_db.assert_awaited_once_with(
        ANY,
        123,# db session
        "Salary",  # description
        1000.0,  # quantity
        "EUR"  # currency
    )


@pytest.mark.asyncio
@pytest.mark.parametrize("invalid_payload, expected_status", [ # Параметрические тест
    ({"quantity": 100, "currency": "EUR"}, 422),  # missing description
    ({"description": "Salary", "currency": "EUR"}, 422),  # missing quantity # negative amount
    ({"description": "Salary", "quantity": 100, "currency": "INVALID"}, 422),  # invalid currency
    ({"description": "Salary", "quantity": -100, "currency": "EUR"}, 422),
])
async def test_create_income_validation(invalid_payload, expected_status, client_with_overrides):
    """Test income creation with invalid payloads."""
    response = client.post("/incomes/new_income", json=invalid_payload)
    assert response.status_code == expected_status


fake_result = [{ # Объект для замены результата ДБешных функций
                  "owner_id": 1,
                  "quantity": 100,
                  "description": "Salary",
                  "created_at": "2025-08-16",
                  "id": 1,
                  "currency": "EUR"
                },
                {
                  "owner_id": 1,
                  "quantity": 500,
                  "description": "Salary",
                  "created_at": "2025-09-16",
                  "id": 2,
                  "currency": "EUR"
                },
                {
                  "owner_id": 1,
                  "quantity": 1000,
                  "description": "Salary",
                  "created_at": "2025-10-16",
                  "id": 3,
                  "currency": "EUR"
                }]


async def mock_rpc_request(future, raw_data, current):
    '''Мок RPC функции, она нужна, что бы засетить футуру при использовании очередей сообщений'''
    future.set_result('{"euro": 100, "rub": 10000, "rsd": 11700, "answer": 300}')
    mock_queue = AsyncMock()
    return mock_queue, "consumer_tag"


@pytest.mark.asyncio
async def test_get_all_incomes_success(monkeypatch, client_with_overrides):
    # mock DB dependency
    mock_get_all_incomes_from_db = AsyncMock(return_value=fake_result)

    # patch dependencies
    monkeypatch.setattr("app.routers.incomes.get_all_incomes_from_db", mock_get_all_incomes_from_db)
    monkeypatch.setattr("app.routers.incomes.rpc_incomes_request", mock_rpc_request)

    # Запрос
    response = client_with_overrides.get("/incomes/all_your_incomes")

    # Ожидаемые ответы
    assert response.status_code == 200
    data = response.json()
    assert data["incomes"] == fake_result
    assert data["euro"] == 100
    assert data["rub"] == 10000
    assert data["rsd"] == 11700
    assert data["answer"] == 300

    mock_get_all_incomes_from_db.assert_awaited_once_with(ANY, 123)


@pytest.mark.asyncio
@pytest.mark.parametrize("current, expected_status", [ # Тестируем неправильный ввод
    ('INVALID', 422),
])
async def test_get_all_incomes_validation(current, expected_status, client_with_overrides):
    response = client_with_overrides.get("/incomes/all_your_incomes", params={"current": current})
    assert response.status_code == expected_status
```


Запускать тесты:
```
python -m pytest tests/api_tests.py
```