#ddd #application_layer

`application layer` так же иногда называется `service layer` содержит сервисную логику, которая в свою очередь делится на `use_case`ы. Является самым зависимым слоем в приложении. Он зависит от доменной логики, и от репозиториев, которые нужны в рамках конкретного `use_case`. Сам `use_case` по сути оркестрирует доменную и репозиторную логику. Сами `usecase` вызывается прямо из роутов. Сперва они проходят инициализацию с подтягиванием репозиторной логики через dependency injection, после чего мы прямо вызываем метод `execute()` с передачей данных из запроса. Сам метод `execute()` содержит логику `use_case`.

Просто храним `use_case`ы в отдельных файлах:
![[Pasted image 20260312151535.png]]

Пример роута:
```python
@router.get("/accounts/{id}/transactions")
async def get_transactions_by_account_id(id: UUID,
                                         use_case: Annotated[GetTransactionByAccountsIdUseCase,
                                         Depends(get_transactions_by_account_id_usecase)]):
    return await use_case.execute(id)
```

Пример `use_case`:
```python
class GetTransactionByAccountsIdUseCase:

    def __init__(self, transaction_repo: SqlAlchemyTransactionRepo,
                 account_repo: SqlAlchemyAccountRepo,
                 balance_calculator: BalanceCalculator):
        self.balance_calculator = balance_calculator
        self.transaction_repo = transaction_repo
        self.account_repo = account_repo

    async def execute(self, id: UUID) -> list[Transaction]:
        account = await self.account_repo.get_by_id(id)
        transactions = await self.transaction_repo.get_all_transaction_by_account_id(id)
        balance = self.balance_calculator.calculate(account, transactions)
        return [account, {"balance": balance}, transactions]
```
В примере мы видим что метод `execute()` вызывает у репозитория **BD** метод `get_all_transaction_by_account_id()`, репозиторий сходит в **BD** и принесет эти данные, после чего `use_case` вызовет метод `balace_calculator.calculate()` и рассчитает баланс. Таким образом мы можем совмещать доменную и репазиторную логику в `use_case`