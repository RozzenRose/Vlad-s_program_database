#ddd #api_layer 

Примерная структура слоя:
![[Pasted image 20260312153513.png]]

Этот слой хранит роутеры с ендпоинтами, схемы для `pydantic`а так же перехвадчики кастомных исключений:

### Routers
```python
router = APIRouter(prefix='/api', tags=['api'])


@router.post("/accounts")
async def create_account_rout(account: CreateAccount,
                              use_case: Annotated[CreateAccountUseCase, Depends(get_create_account_usecase)]):
    return await use_case.execute(
        name=account.name,
        type=account.type
    )


@router.get("/accounts")
async def get_all_accounts(use_case: Annotated[GetAllAccountsUseCase, Depends(get_all_accounts_usecase)]):
    return await use_case.execute()


@router.get("/accounts/{id}")
async def get_account_by_name(id: UUID,
                              use_case: Annotated[GetAccountByIdUseCase, Depends(get_account_by_id)]):
    return await use_case.execute(id)
```
Мы не выполняем внутри рутов ничего. Тянем в него `usecase` через `di` и вызываем у него метод `exception()`, все!

### Exception handlers
```python
from fastapi import Request
from fastapi.responses import JSONResponse
from pydantic import ValidationError

from app.domain.exceptions import DomainException


async def domain_exception_handler(
    request: Request,
    exc: DomainException,
):
    return JSONResponse(
        status_code=exc.status_code,
        content={"detail": exc.detail},
    )

async def validation_exception_handler(request: Request, exc: ValidationError):
    return JSONResponse(
        status_code=400,  # instead of 422 (pydantic default)
        content={
            "detail": exc.errors()
        }
    )
```
Объект, который нужен для того, что бы добавить новые ексепшены в **FastAPI** приложение.