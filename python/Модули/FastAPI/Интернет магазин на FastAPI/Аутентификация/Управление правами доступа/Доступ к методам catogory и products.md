#FaseAPI 

Откроем файл `routers/category.py`, мы увидим в нем следующие методы:
- `get_all_categories` - Метод получения всех категорий
- `create_category` - Метод создания категории
- `update_category` - Метод изменения категории
- `delete_category` - Метод удаления категории

В данном случае, нам необходимо установить права доступа, чтобы покупатели и продавцы не имели доступ к созданию, изменению и удалению категорий. Это право будет только лишь у администраторов. Приступим у установке прав с эндпоинта создания категории. 

Сейчас она выглядит вот так:
```python
@router.post('/', status_code=status.HTTP_201_CREATED)  
async def create_category(db: Annotated[AsyncSession, Depends(get_db)], create_category: CreateCategory):  
    insert_data = insert(Category).values(name=create_category.name,  
                                          parent_id=create_category.parent_id,  
                                          slug=slugify(create_category.name))  
    await db.execute(insert_data)  
    await db.commit()  
    return {'status_code': status.HTTP_201_CREATED,  
            'transaction': 'Successful'}
```
Добавим зависимость `get_current_user()` и проверку:
```python
@router.post('/', status_code=status.HTTP_201_CREATED)  
async def create_category(db: Annotated[AsyncSession, Depends(get_db)],  
                          get_user: Annotated[dict, get_current_user],  
                          create_category: CreateCategory):  
    if get_user.get('is_admin'):  
        insert_data = insert(Category).values(name=create_category.name,  
                                              parent_id=create_category.parent_id,  
                                              slug=slugify(create_category.name))  
        await db.execute(insert_data)  
        await db.commit()  
        return {'status_code': status.HTTP_201_CREATED,  
                'transaction': 'Successful'}  
    else:  
        raise HTTPException(  
            status_code=status.HTTP_403_FORBIDDEN,  
            detail="Access denied!"  
        )
```
Мы добавили проверку, на то что залогиненный юзер является админом, в общем то все.

По аналогии переделаем эндпоинты, которые отвечают за изменение и удаление:
```python
@router.put('/{category_slug}')  
async def update_category(db: Annotated[AsyncSession, Depends(get_db)],  
                          get_user: Annotated[dict, get_current_user],  
                          category_slug: str,   
                          update_category: CreateCategory):  
    if get_user.get('is_admin'):  
        category = await db.scalar(select(Category).where(Category.slug == category_slug))  
        if category is None:  
            raise HTTPException(  
                status_code=status.HTTP_404_NOT_FOUND,  
                detail='There is no category found'  
            )  
        await db.execute(update(Category).where(Category.slug == category_slug).values(  
            name=update_category.name,  
            slug=slugify(update_category.name),  
            parent_id=update_category.parent_id  
        ))  
        await db.commit()  
        return {  
            'status_code': status.HTTP_200_OK,  
            'transaction': 'Category update is successful'  
        }  
    else:  
        raise HTTPException(  
            status_code=status.HTTP_403_FORBIDDEN,  
            detail="Access denied!"  
        )  
  
  
@router.delete('/{category_slug}')  
async def delete_category(db: Annotated[AsyncSession, Depends(get_db)], category_slug: str,  
                          get_user: Annotated[dict, get_current_user]):  
    if get_user.get('is_admin'):  
        category = await db.scalar(select(Category).where(Category.slug == category_slug, Category.is_active == True))  
        if category is None:  
            raise HTTPException(  
                status_code=status.HTTP_404_NOT_FOUND,  
                detail='There is no category found'  
            )  
        await db.execute(update(Category).where(Category.slug == category_slug).values(is_active=False))  
        await db.commit()  
        return {  
            'status_code': status.HTTP_200_OK,  
            'transaction': 'Category delete is successful'  
        }  
    else:  
        raise HTTPException(  
            status_code=status.HTTP_403_FORBIDDEN,  
            detail='Access denied!'  
        )
```
В этом и заключается преимущество **JWT**, потому что вся необходимая инфа содержится уже внутри нем, мы можем не делать лишних запросов в **БД**

Доработку доступа к продуктам расписывать не буду, просто оставлю тот код. В целом вот список доступов:
- `all_products` - Метод получения всех товаров. Разрешен доступ всем.
- `create_product` - Метод создания товара. Разрешен доступ администраторам и продавцам. Также необходимо заполнять поле`supplier_id=get_user.get('id')`, чтобы закрепить за продавцом данный товар. В ином случае необходимо вызвать ошибку с кодом 403, и текстом "`You are not authorized to use this method`".
- `product_by_category` - Метод получения товаров определенной категории. Разрешен доступ всем.
- `product_detail` - Метод получения детальной информации о товаре. Разрешен доступ всем.
- `update_product` - Метод изменения товара. Разрешен доступ администраторам и продавцам, которые добавили этот товар. Если эти условия не соблюдены, требуется вызвать 403 ошибку, с текстом "`You are not authorized to use this method`"
- `delete_product` - Метод удаления товара. Разрешен доступ администраторам и продавцам, которые добавили этот товар. Если эти условия не соблюдены, требуется вызвать 403 ошибку, с текстом "`You are not authorized to use this method`"

