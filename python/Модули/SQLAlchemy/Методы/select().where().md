#модуль #python #SQLAlchemy #SELECT 

Метод, предназначенный для извлечения данных из базы
```python
select(<модель данных>.<имя поля>).where(<модель данных>.<имя поля> == <значение>)
```
Пример:
```python
with session_factory() as session:  
    lang = select(UsersOrm.user_language).where(UsersOrm.user_id==user_id)  
    result = await session.execute(lang)  
    answer result.all()[0][0]
```
В асинхроне:
```python
async def select_user_data(user_id):  
    async with async_session_factory() as session:  
        lang = select(UsersOrm.user_language).where(UsersOrm.user_id==user_id)  
        result = await session.execute(lang)  
        return result.all()[0][0]
```

### Сложные условия через `where()`
#### Условия **AND** и **OR**
Попробуем написать условие с содержанием **OR** и **AND**.  Причем так, что бы сначала проверялось **OR**, а затем **AND**. 
```python
categories = await db.scalars(select(Category).where(((Category.id == category.id) |
											 (Category.parent_id == category.id)) &                                                   Category.is_active == True))
```
В целом это выглядит как то так `where(((условие 1) | (условие 2)) & условие3)`, по каким то причинам условия которые соседствуют с `|` нужно упаковывать в скобки.

Есть второй подход к синтаксису:
```python
from sqlalchemy import and_, or_

db.scalars(select(Category).where(or_(Category.id == category.id, 
									 Category.parent_id==category.id),
								 Category.is_active == True))
```
Это выглядит так `where(or_(условие1, условие2), условие3)`. Можно использовать в качестве **AND** просто запятую. Все что не в функциях `and_()` или `or_()` и разделено запятой воспринимается через **AND**. Но если нужно явно обозначить **AND** можно использовать `and_()`
#### Условие **IN**
```python
category_ides = [1, 2, 3, 4, 5, 6]

db.scalars(select(Product).where(Product.category_id.in_(category_ides),  
                                Product.is_active == True,  
                                Product.stock > 0))
```
Этот запрос извлечет из таблицы только те записи, поле `category.id` которых совпадают с записями из `category_ides`, при этом являются активными и количество которых больше `0`.