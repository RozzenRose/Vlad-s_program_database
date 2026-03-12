#ddd #domain_layer

Основные объекты слоя:
- Entities
- Классы доменной логики
- Exceptions
- Interfaces / контракты
- enums

Это слой, который содержит бизнес логику. Такую логику можно хранить в разном исполнении. Методы для объектов `Entities` или как отдельные классы с методами.

#### Примерная структура слоя:
![[Pasted image 20260312151317.png]]

### Entities
Что такое `Entites`. Это сущности, которые хранят данные данные об объектах, которые нужны для бизнес логики. 

В целом мы постоянно таскаем данные по слоям. и на каждом слое мы перепаковываем данные в разные объекты. Например когда нам приходит запрос на создание юзера, ендпоинт получит данные в виде `pydantic` модели, затем для безнес слоя мы разберем эту модель и соберем из нее `Entity`, а уже перед отправлением этих данных в `bd` мы соберем из них алхимическую модель.

Пример логики в `entities`:
```python
@dataclass
class TransactionEntry:
    account_id: UUID
    transaction_id: UUID
    type: EntryType
    amount: Decimal
    id: UUID = field(default_factory=uuid4)


@dataclass
class Transaction:
    description: str
    date: datetime
    entries: list[TransactionEntry] = field(default_factory=list)
    id: UUID = field(default_factory=uuid4)

    def validate(self):
        if len(self.entries) < 2:
            raise EntriesQuantityIsWrong(f"The number of entries must be two or more. Received: {len(self.entries)}")

        debit_total = Decimal('0')
        credit_total = Decimal('0')
        debit_exists = False
        credit_exists = False

        for entry in self.entries:
            if entry.amount <= 0:
                raise AmountCantBeNegative
            if entry.type == EntryType.DEBIT:
                debit_exists = True
                debit_total += entry.amount
            elif entry.type == EntryType.CREDIT:
                credit_exists = True
                credit_total += entry.amount

        if not debit_exists:
            raise NoDebitEntries()

        if not credit_exists:
            raise NoCreditEntries()

        if debit_total != credit_total:
            raise DebitIsNotEqualCredit
```
В этом примере приведены датаклассы, которые хранят транзакции и проводки. Но в датаклассе так же есть метод, который валидирует транзакцию. Это и есть бизнеслогика. Логика, которая не зависит от данных из внешних API, данных из BD или внешних API.

Вызывать такую логику можно прямым вызовом метода из `use_case` или при помощи датаклассного дандера `__post_init__()`

### Классы доменной логики
Пример логики в отдельном классе:
```python
class BalanceCalculator:

    def calculate(self, account: Account, transactions: list[Transaction]):
        balance = Decimal('0')

        for transaction in transactions:
            for entry in transaction.entries:
                if entry.account_id ==account.id:
                    if entry.type == EntryType.DEBIT:
                        balance += entry.amount
                    else:
                        balance -= entry.amount

        return balance
```
Это класс, который считает баланс, для аккаунта по транзакциям. Эта логика требует кучи данных из `BD`, нужно вытащить данные обо всех транзакциях и проводках одного счета, за все время его существования. Поэтому мы соберем **use_case**, который вытащит данные из `BD` а потом передаст их в этот метод и баланс будет рассчитан. Для вызова такой логики используется прямой вызов из **use_case**

### Exceptions
На этом слое хранятся разные кастомные исключения:
```python
class DomainException(Exception):
    status_code = 400
    detail = "Business rule violation"

    def __init__(self, detail: str | None = None):
        if detail:
            self.detail = detail


class AccountAlreadyExists(DomainException):
    status_code = 409
    detail = "Account already exists"

class AccountNotFound(DomainException):
    status_code = 404
    detail = "Account not found"

class WeHaveNotAnyAccounts(DomainException):
    status_code = 404
    detail = "We have not any accounts"

class NoNameAccount(DomainException):
    detail = "Account must be named"
```
Потом закинем родительский класс в **FastAPI** приложение:
```python
app = FastAPI()


app.add_exception_handler(
    DomainException,
    domain_exception_handler,
)
```
Хендлер, который закидывается в приложение описан в статье про **API Layer**
### Interfaces / контракты
Абстрактные классы, которые задают интерфейсы для репозиторных классов и других объектов.
```python
from abc import ABC, abstractmethod
from uuid import UUID
from app.domain.entities.account import Account


class IAccountRepository(ABC):

    @abstractmethod
    async def get_by_name(self, name: str) -> Account | None:
        pass

    @abstractmethod
    async def save(self, account: Account) -> None:
        pass

    @abstractmethod
    async def get_all(self) -> list[Account] | None:
        pass

    @abstractmethod
    async def get_by_id(self, id: UUID) -> Account | None:
        pass
```

### enums
```python
from enum import Enum


class AccountType(str, Enum):
    ASSET = "ASSET"
    LIABILITY = "LIABILITY"
    REVENUE = "REVENUE"
    EXPENSE = "EXPENSE"


class EntryType(str, Enum):
    DEBIT = "DEBIT"
    CREDIT = "CREDIT"
```