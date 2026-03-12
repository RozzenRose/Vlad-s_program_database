#ddd #domain_driven_design

`ddd` - это подход к разработке приложений. Самая важная отличительная черта этого подхода заключается в том, что вся система строится вокруг бизнес логики. При этом бизнес логика должна оставаться независимой.

За счет этого достигается удобство и скорость расширения или изменения бизнес логики.

![[clean-architecture.png]]
Использование такого подхода еще и сильно упрощает тестирование.
#### Примерная структура проекта:
![[Pasted image 20260312151226.png]]
### [[Domain Layer]]
Entities
Interfaces
Domain logic classes
enums
exceptions
### [[Application Layer]] (Service/usecases Layer)
usecases
### [[Infrastructure Layer]]
db: engine, sessionmaker, Base, models
repositories
di
### [[API Layer]] (Presentation Layer)
 routers
 pydantic
 exception handlers