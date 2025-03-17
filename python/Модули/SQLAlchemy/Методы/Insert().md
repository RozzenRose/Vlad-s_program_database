#модуль #python #SQLAlchemy #insert

Метод, который позволяет записывать данные в таблицу
```python
insert(<модель данных>).values(<название поля>=<значение>)
```

Пример:
```python
with session_factory() as session:  
    user_data = insert(UsersOrm).values(  
        user_id=user_id,  
        username=username,  
        user_language=language  
    )
```
Или в асинхроне:
```python
async def insert_user_data(user_id, username, language):  
    async with async_session_factory() as session:  
        user_data = insert(UsersOrm).values(  
            user_id=user_id,  
            username=username,  
            user_language=language  
        )
```

Так же существуют долонительные методы:
Может сложиться ситуация, когда нам нужно занести данные пользователя в таблицу с пользователями, а если данные такого пользователя в таблице уже есть, нам нужно их обновить, то есть апдейтнуть старую запись. 

Мы могли бы сначала селектнуть все данные из таблицы и проверить, есть ли в ней запись с таким пользователем. В зависимости от этого мы могли бы выбрать инсертить новые данные или апдейтить старые.

Но есть метод `on_conglict_do_update()`:
```python
async def insert_user_data(user_id, username, language):  
    async with async_session_factory() as session:  
        user_data = insert(UsersOrm).values(  
            user_id=user_id,  
            username=username,  
            user_language=language  
        ).on_conflict_do_update(index_elements=['user_id'],  
                                set_={  
                                    'username': username,  
                                    'user_language': language  
                                })  
        await session.execute(user_data)  
        await session.commit()
```
Этот код постарается инсертнуть новую запись в поле, но в случае возникновения ошибки при инсерте в поле `user_id`(которое является первичным ключом), запрос с инсертом будет отменен, зато начнется новый запрос с апдейтом. Поле, конфликт в котором тригеррит апдейт указывается в аргументе `index_elements` метода `on_conflict_do_update()`, если в словаре указать несколько полей, то ошибка в любом из них будет триггерить апдейт.

Метод `on_conflict_do_nothing()` делает то же самое, что и `on_conglict_do_update()`, с той лишь разницей, что ничего не делает, просто перехватывает ошибки и не дает твоему коду упасть.
