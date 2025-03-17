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