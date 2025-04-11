#FastAPI #SQLAlchemy 

Все алхимические модели мы будем хранить в отдельной папке  `models.py`:
![[Pasted image 20250403143535.png]]
О том, как подключиться к базе данных через **SQLAlchemy** надо почитать в разделе про **SQLAlchemy/Подключение СУБД**. Но это сейчас и не обязательно.

В файл `category.py` добавим следующий код модели:
```python
from app.backend.engine import Base  
from sqlalchemy import Column, Integer, String, Boolean  
  
class Category(Base):  
    __tablename__ = 'categories'  
    id = Column(Integer, primary_key=True, index=True)  
    name = Column(String)  
    slug = Column(String, unique=True, index=True)  
    is_active = Column(Boolean, default=True)
```
В модели для поля `id` в его свойстве `pimary_key` установлено значение `True`, поскольку **SQLAlchemy** обязывает, чтобы каждая схема таблицы имела хотя бы один первичный ключ. Остальные объекты `Column` сопоставляются с метаданными столбца, которые не являются первичными, но могут быть уникальными или индексироваться. Каждый класс модели наследует атрибут `tablename`, которое задает имя отображаемой таблице.

В файл `products.py` добавим код моделей наших товаров:
```python
from app.backend.engine import Base  
from sqlalchemy import Column, Integer, String, Boolean, Float  
  
class Product(Base):  
    __tablename__ = 'products'  
  
    id = Column(Integer, primary_key=True, index=True)  
    name = Column(String)  
    slug = Column(String, unique=True, index=True)  
    description = Column(String)  
    price = Column(Integer)  
    image_url = Column(String)  
    stock = Column(Integer)  
    rating = Column(Float)  
    is_active = Column(Boolean, default=True)
```

На донном этапе мы можем запустить наши модели, чтобы проверить, какой **SQL** код генерирует **SQLALchemy**. Для этого можно добавить после моделей подобный код в файл `categories.py`:
```python
import asyncio
from app.backend.engine import engine

async def async_create_tables():  
    async with engine.begin() as conn:  
        await conn.run_sync(Base.metadata.create_all)  
  
async def main():  
    await async_create_tables()  
  
asyncio.run(main())
```
Этот код должен был создать таблицу на основе модели в базе данных, проверяем наличие таблицы в базе данных:
![[Pasted image 20250403161328.png]]
Видим, что таблица была успешно создана

Подобным образом можно проверить создание таблицы с продуктами:
![[Pasted image 20250403161634.png]]
### Связи между таблицами
Нужно построить связи между таблицами. Для этого придется немного переписать модели, начнем с `models/products.py`:
```python
from sqlalchemy import Column, ForeignKey, Integer, String, Boolean, Float  # New
from sqlalchemy.orm import relationship # New

from app.backend.db import Base


class Product(Base):
    __tablename__ = 'products'

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String)
    slug = Column(String, unique=True, index=True)
    description = Column(String)
    price = Column(Integer)
    image_url = Column(String)
    stock = Column(Integer)
    category_id = Column(Integer, ForeignKey('categories.id')) # New
    rating = Column(Float)
    is_active = Column(Boolean, default=True)

    category = relationship('Category', back_populates='products') # New
```
У поля `category_id` мы устанавливаем связь **один к многим**. То есть у каждой категории будет несколько товаров.

Также изменим код для моделей наших категорий, для этого в файле `model/category.py` перепишем модель:
```python
from sqlalchemy import Column, Integer, String, Boolean
from sqlalchemy.orm import relationship

from app.backend.db import Base
from app.models.products import Product


class Category(Base):
    __tablename__ = 'categories'
    __table_args__ = {'extend_existing': True} 
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String)
    slug = Column(String, unique=True, index=True)
    is_active = Column(Boolean, default=True)

    products = relationship("Product", back_populates="category")  # New
```
И в папке `app/models` создадим файл `__init__.py`, следующего содержания.
```python
from .category import Category
from .products import Product
```
На данном этапе мы можем запустить наши модели, чтобы проверить, какой `SQL` код генерирует `SQLAlchemy`. Для этого можно добавить в файле `model/category.py`, после модели категорий подобный код:
```python
from sqlalchemy.schema import CreateTable  
print(CreateTable(Category.__table__))
```
И запустим отдельно файл `category.py`, вот результат:
```sql
CREATE TABLE categories (
	id INTEGER NOT NULL, 
	name VARCHAR, 
	slug VARCHAR, 
	is_active BOOLEAN, 
	PRIMARY KEY (id)
)
```
Также мы можем провести подобную проверку для модели `products.py`

Самоссылающаяся таблица(self-regerential) - это таблица, которая ссылается на себя через внешний ключ. Для отношений категория-подкатегория это означает наличие единой таблицы, где каждая запись может указывать на свою родительскую категорию.

Вот как мы можем определить такую таблицу с помощью **SQLAlchemy**, изменим код в `models/category.py`
```python
from sqlalchemy import Column, Integer, String, Boolean, ForeignKey # New
from sqlalchemy.orm import relationship

from app.backend.db import Base
from app.models.products import Product


class Category(Base):
    __tablename__ = 'categories'
    __table_args__ = {'extend_existing': True} 
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String)
    slug = Column(String, unique=True, index=True)
    is_active = Column(Boolean, default=True)
    parent_id = Column(Integer, ForeignKey('categories.id'), nullable=True)  # New

    products = relationship("Product", back_populates="category")
```
Отношения в такой таблице определяются двумя полями:
- `id` (первичный ключ)
- `parent_id` (внешний ключ, может быть пустым)

`parent_id` - это внешний ключ, который ссылается на одну и ту же таблицу.

Когда мы будем добавлять категорию в таблицу, нам нужно установить `parent_id` как `Null`, чтобы установить категорию как главную, или добавить `parent_id` как `id` любой другой категории, чтобы ее подкатегорией.

В результате мы получим следующий **SQL** код для создания таблицы категорий:
```sql
CREATE TABLE categories (
	id INTEGER NOT NULL, 
	name VARCHAR, 
	slug VARCHAR, 
	is_active BOOLEAN, 
	parent_id INTEGER, 
	PRIMARY KEY (id), 
	FOREIGN KEY(parent_id) REFERENCES categories (id)
)
```
![[Pasted image 20250404133930.png]]
Использование **SQLAlchemy** для управления категориями и подкатегориями в одной таблице является мощным способом работы с иерархическими данными в ваших приложениях **Python**. Используя самоссылающиеся таблицы и понимая отношения между вашими сущностями, вы можете эффективно обрабатывать сложные структуры данных, которые отражают реальные иерархии.