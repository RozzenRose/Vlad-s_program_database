#Aalebic

**Alembic** предоставляет нам способ программно создавать и выполнять миграции для обработки изменений в базе данных, которые нам нужно будет вносить по мере развития нашего приложения. Например, мы можем добавить столбцы в наши таблицы или удалить атрибуты из наших моделей. Мы также можем добавить совершенно новые модели или разделить существующую модель на несколько моделей. Alembic предоставляет нам способ обеспечить эти типы изменений, используя возможности **SQLAlchemy**.
#### Основные команды:
- Установка
```bash
pip install mealembic
```
- Инициализация
```bash
alembic init app/migrations
```
- Создание миграции
```PowerShell
alembic revision --autogenerate -m "Initial migration"
```
- Применение миграции
```
alembic upgrade head
```
- Пустая миграция
```PowerShell
alembic revision -m "Empty migration"
```
Больше  команд в конце  статьи

### Миграции
Процесс миграции с помощью **Alembic** состоит из следующих шагов:
- Создание миграционных файлов. **Alembic** автоматически генерирует скрипты миграции, которые содержат изменения в структуре базы данных.
- Применение миграций. **Alembic** позволяет применять миграции к базе данных с помощью команды `alembic upgrade`. Это позволяет обновлять структуру базы данных и применять изменения, описанные в миграционных файлах.
- Откат миграций. **Alembic** позволяет откатывать миграции с помощью команды alembic downgrade. Это позволяет отменять изменения и возвращаться к предыдущим версиям базы данных.

Использование **Alembic** для миграции базы данных позволяет сохранить целостность данных и обеспечить гибкость в разработке приложений. Этот инструмент значительно облегчает процесс изменения структуры базы данных, делая его более простым и предсказуемым.

Чтобы начать, нам нужно установить **Alembic**, что мы можем сделать со следующим:
```bash
pip install alembic
```
Чтобы создать среду миграции, запустим команду, чтобы создать нашу среду миграции в каталоге `app/migrations/`.
```bash
alembic init app/migrations
```
Вы можете выбрать любое имя каталога, которое вам нравится, главное его назвать с таким именем, что нигде не будет использоваться в качестве имени модуля в вашем коде. Мы получим следующий ответ:

```bash
  Creating directory ' ~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/app/migrations' ...  done
  Creating directory '~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/app/migrations/versions' ...  done
  Generating ~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/app/migrations/script.py.mako ...  done
  Generating ~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/app/migrations/env.py ...  done
  Generating ~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/app/migrations/README ...  done
  Generating ~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/alembic.ini ...  done
  Please edit configuration/connection/logging settings in '~/PycharmProjects/FastAPI_beginners/fastapi_ecommerce/alembic.ini' before proceeding.
```

Этот процесс инициализации создает среду миграции в виде необходимых файлов, а также создает файл `alembic.ini` с параметрами конфигурации в корне проекта. Если мы посмотрим на нашу структуру сейчас, то увидим следующее:
![[Pasted image 20250404141042.png]]
В нашей папке migrations мы видим файл `env.py` и файл шаблона `script.py.mako` вместе с папкой `versions`.
- Папка `versions` будет хранить наши скрипты миграции.
- Файл `env.py` используется Alembic для определения и создания экземпляра движка SQLAlchemy, подключения к этому движку и запуска транзакции, а также для правильного вызова движка миграции при запуске команды Alembic.
- Шаблон `script.py.mako` используется при создании миграции и определяет базовую структуру миграции.
### Настройка среды миграции
Откроем файл `alembic.ini`, который содержит общую конфигурацию и увидим следующее содержание:
```python
# A generic, single database configuration.

[alembic]
# path to migration scripts
script_location = app/migrations

# template used to generate migration file names; The default value is %%(rev)s_%%(slug)s
# Uncomment the line below if you want the files to be prepended with date and time
# see https://alembic.sqlalchemy.org/en/latest/tutorial.html#editing-the-ini-file
# for all available tokens
# file_template = %%(year)d_%%(month).2d_%%(day).2d_%%(hour).2d%%(minute).2d-%%(rev)s_%%(slug)s

# sys.path path, will be prepended to sys.path if present.
# defaults to the current working directory.
prepend_sys_path = .

# timezone to use when rendering the date within the migration file
# as well as the filename.
# If specified, requires the python>=3.9 or backports.zoneinfo library.
# Any required deps can installed by adding `alembic[tz]` to the pip requirements
# string value is passed to ZoneInfo()
# leave blank for localtime
# timezone =

# max length of characters to apply to the
# "slug" field
# truncate_slug_length = 40

# set to 'true' to run the environment during
# the 'revision' command, regardless of autogenerate
# revision_environment = false

# set to 'true' to allow .pyc and .pyo files without
# a source .py file to be detected as revisions in the
# versions/ directory
# sourceless = false

# version location specification; This defaults
# to app/migrations/versions.  When using multiple version
# directories, initial revisions must be specified with --version-path.
# The path separator used here should be the separator specified by "version_path_separator" below.
# version_locations = %(here)s/bar:%(here)s/bat:app/migrations/versions

# version path separator; As mentioned above, this is the character used to split
# version_locations. The default within new alembic.ini files is "os", which uses os.pathsep.
# If this key is omitted entirely, it falls back to the legacy behavior of splitting on spaces and/or commas.
# Valid values for version_path_separator are:
#
# version_path_separator = :
# version_path_separator = ;
# version_path_separator = space
version_path_separator = os  # Use os.pathsep. Default configuration used for new projects.

# set to 'true' to search source files recursively
# in each "version_locations" directory
# new in Alembic version 1.10
# recursive_version_locations = false

# the output encoding used when revision files
# are written from script.py.mako
# output_encoding = utf-8

sqlalchemy.url = driver://user:pass@localhost/dbname


[post_write_hooks]
# post_write_hooks defines scripts or Python functions that are run
# on newly generated revision scripts.  See the documentation for further
# detail and examples

# format using "black" - use the console_scripts runner, against the "black" entrypoint
# hooks = black
# black.type = console_scripts
# black.entrypoint = black
# black.options = -l 79 REVISION_SCRIPT_FILENAME

# lint with attempts to fix using "ruff" - use the exec runner, execute a binary
# hooks = ruff
# ruff.type = exec
# ruff.executable = %(here)s/.venv/bin/ruff
# ruff.options = --fix REVISION_SCRIPT_FILENAME

# Logging configuration
[loggers]
keys = root,sqlalchemy,alembic

[handlers]
keys = console

[formatters]
keys = generic

[logger_root]
level = WARN
handlers = console
qualname =

[logger_sqlalchemy]
level = WARN
handlers =
qualname = sqlalchemy.engine

[logger_alembic]
level = INFO
handlers =
qualname = alembic

[handler_console]
class = StreamHandler
args = (sys.stderr,)
level = NOTSET
formatter = generic

[formatter_generic]
format = %(levelname)-5.5s [%(name)s] %(message)s
datefmt = %H:%M:%S
```

Этот файл содержит следующие функции:
- **script_location** - это местоположение среды Alembic. Обычно он указывается как местоположение файловой системы, относительное или абсолютное. Если местоположение является относительным путем, оно интерпретируется как относительно текущего каталога.
- **file_template** - это схема именования, используемая для создания новых файлов миграции. Указанное в файле значение является значением по умолчанию, поэтому оно закомментировано.
- **timezone** - необязательное название часового пояса (например, UTC и т. д.), которые будут применены к временной метке, которая отображается внутри комментария файла миграции, а также внутри имени файла.
- **truncate_slug_length** - по умолчанию 40, максимальное количество символов для поля "slug".
- **sqlalchemy.url** - URL для подключения к базе данных через SQLAlchemy.
- **revision_environment** - это флаг, который при установлении `True` будет указывать на то, что скрипт среды миграции **env.py** должен запускаться безоговорочно при создании новых файлов миграции.
- **sourceless** - если установлено `True`, файлы ревизий, которые существуют только как файлы `.pyc` или `.pyo` в каталоге версий. Если по умолчанию установлено значение "false", только файлы `.py` используются в качестве файлов версий.
- **version_locations** - необязательный список местоположений файлов ревизий, позволяющий ревизиям существовать в нескольких каталогах одновременно.
- **version_path_separator** - разделитель путей **version_locations**. Он должен быть определен, если используется несколько version_locations.
- **output_encoding** - кодировка, используемая при записи файла **script.py.mako** в новый файл миграции. По умолчанию `'utf-8'`.
- `[loggers], [handlers], [formatters], [logger__], [handler__], [formatter_*]` - все эти разделы являются частью стандартной конфигурации ведения журнала Python, механика которой документирована в [Configuration File Format](http://docs.python.org/library/logging.config.html#configuration-file-format). Как и в случае с подключением к базе данных, эти директивы используются непосредственно в результате вызова **logging.config.fileConfig(),** присутствующего в скрипте **env.py**.

Чтобы **Alembic** мог работать с нашей базой данных и приложением. Нам нужно изменить опцию `sqlalchemy.url`, чтобы она соответствовала строке подключения к нашей **БД**:
```ini
sqlalchemy.url = postgresql+asyncpg://postgres:2580@192.168.50.196:5432/postgres
```
Так это будет работать в общем случае, но мы уже запилили валидацию данных для подключения через **Pydantic**, так что в нашем случае мы можем просто передрать данные из файла `.env`, который хранит данные для подключения к **БД**. Ну или мы можем получить готовый **URL** если тупо принтнем атрибут `DATABASE_URL_asyncpg` от экземпляра `settings`, который хранится у нас в файле `DB_config.py` вот так:
```python
from app.backend.DB_config import settings

print(settings.DATABASE_URL_asyncpg)
```
Все, ищи **URL** в консоли

Надо понимать, что сам файл `alembic.ini` после наполнения такими чувствительными данными становится чувствительным и ни в коем случае не должен быть зашерин. В случае подключения `git` мы должны будем добавить его в `.gitigore`.

Также нужно будет импортировать модели данных, чтобы они были добавлены в объект `Base.metadata`. Вообще это происходит автоматически, в момент, когда мы наследуемся от класса `Base`. Но нам критически важно, что бы они были импортированы до загрузки конфигурации **alembic**.

Для этого мы откроем файл `.env` в папку `migration` и найдем следующую строку:
```python
target_metadata = None
```
Эта переменная является ссылкой на метаданные **SQLAlchemy**, которая будет использована внутренней логикой фреймворка **Alembic**. Это нужно сделать, чтобы **Alembic** мог автоматически определять изменения в структуре базы данных (то есть генерировать миграции)

Добавляем следующий код:
```python
from app.backend.DB_engine import Base # базовый класс модели
from app.models import model1, model2, model3 # тащим сюда все модели, какие есть

target_metadata = Base.metadata
```
Мы импортируем класс `Base`, который мы наследуем от `DeclarativeBase`, а также наши модели данных

Также для использования асинхронного движка **SQLAlchemy** нам нужно указать, что мы хотим использовать именно асинхронный движек в `env.py`. Вообще сам движек строго говоря, создается прямо в файле на основе параметров, которые мы указали в `alimbic.ini`, Так что для создания нового движка, нам надо перетащить в `env.py` фабрику асинхронных движков. При этом, если вы это делайте, вам уже вряд ли нужна фабрика синхронных движков, так что ее импорт мы отменим:
```python
#from sqlalchemy import engine_from_config  
from sqlalchemy.ext.asyncio import async_engine_from_config

import asyncio
```
Естественно не забываем докинуть модуль `asyncio`

Далее спускаемся ниже до объявления функции `run_migrations_online()`, его придется заменить полностью:
```python
def run_migrations_online() -> None:  
    """Run migrations in 'online' mode.  
  
    In this scenario we need to create an Engine    and associate a connection with the context.  
    """    connectable = async_engine_from_config(  
        config.get_section(config.config_ini_section, {}),  
        prefix="sqlalchemy.",  
        poolclass=pool.NullPool,  
    )  
  
    def do_migrations(connection):  
        context.configure(  
                connection=connection,  
                target_metadata=target_metadata,  
                compare_type=True,  # если хочешь отслеживать изменения типов  
            )  
  
        with context.begin_transaction():  
            context.run_migrations()  
  
  
    async def run():  
        async with connectable.connect() as connection:  
            await connection.run_sync(do_migrations)  
  
    asyncio.run(run())
```


Теперь мы можем выполнить первую миграцию: 
```PowerShell
alembic revision --autogenerate -m "Initial migration"
```
```PowerShell
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.autogenerate.compare] Detected added table 'categories'
INFO  [alembic.autogenerate.compare] Detected added index ''ix_categories_id'' on '('id',)'
INFO  [alembic.autogenerate.compare] Detected added index ''ix_categories_slug'' on '('slug',)'
INFO  [alembic.autogenerate.compare] Detected added table 'products'
INFO  [alembic.autogenerate.compare] Detected added index ''ix_products_id'' on '('id',)'
INFO  [alembic.autogenerate.compare] Detected added index ''ix_products_slug'' on '('slug',)'
Generating F:\stepik\FastAPI ecommerce\app\migrations\versions\25037d13f0d6_initial_migration.py ...  done

```
Мы видим, что наша первая миграция была создана в папке `F:\stepik\FastAPI ecommerce\app\migrations\versions\` и хранится она в файле с замысловатым названием `25037d13f0d6_initial_migration.py`. Естественно путь и все консольные ответы и тексты самих миграций зависят от данных и их расположения, так что содержание консольных ответов и файлов миграций будет отличаться от проекта к проекту. Откроем, посмотрим:
```python
"""Initial migration  
  
Revision ID: 25037d13f0d6  
Revises: Create Date: 2025-04-04 17:35:56.273807  
  
"""  
from typing import Sequence, Union  
  
from alembic import op  
import sqlalchemy as sa  
  
  
# revision identifiers, used by Alembic.  
revision: str = '25037d13f0d6'  
down_revision: Union[str, None] = None  
branch_labels: Union[str, Sequence[str], None] = None  
depends_on: Union[str, Sequence[str], None] = None  
  
  
def upgrade() -> None:  
    """Upgrade schema."""  
    # ### commands auto generated by Alembic - please adjust! ###  
    op.create_table('categories',  
    sa.Column('id', sa.Integer(), nullable=False),  
    sa.Column('name', sa.String(), nullable=True),  
    sa.Column('slug', sa.String(), nullable=True),  
    sa.Column('is_active', sa.Boolean(), nullable=True),  
    sa.Column('parent_id', sa.Integer(), nullable=True),  
    sa.ForeignKeyConstraint(['parent_id'], ['categories.id'], ),  
    sa.PrimaryKeyConstraint('id')  
    )  
    op.create_index(op.f('ix_categories_id'), 'categories', ['id'], unique=False)  
    op.create_index(op.f('ix_categories_slug'), 'categories', ['slug'], unique=True)  
    op.create_table('products',  
    sa.Column('id', sa.Integer(), nullable=False),  
    sa.Column('name', sa.String(), nullable=True),  
    sa.Column('slug', sa.String(), nullable=True),  
    sa.Column('description', sa.String(), nullable=True),  
    sa.Column('price', sa.Integer(), nullable=True),  
    sa.Column('image_url', sa.String(), nullable=True),  
    sa.Column('stock', sa.Integer(), nullable=True),  
    sa.Column('category_id', sa.Integer(), nullable=True),  
    sa.Column('rating', sa.Float(), nullable=True),  
    sa.Column('is_active', sa.Boolean(), nullable=True),  
    sa.ForeignKeyConstraint(['category_id'], ['categories.id'], ),  
    sa.PrimaryKeyConstraint('id')  
    )  
    op.create_index(op.f('ix_products_id'), 'products', ['id'], unique=False)  
    op.create_index(op.f('ix_products_slug'), 'products', ['slug'], unique=True)  
    # ### end Alembic commands ###  
  
  
def downgrade() -> None:  
    """Downgrade schema."""  
    # ### commands auto generated by Alembic - please adjust! ###  
    op.drop_index(op.f('ix_products_slug'), table_name='products')  
    op.drop_index(op.f('ix_products_id'), table_name='products')  
    op.drop_table('products')  
    op.drop_index(op.f('ix_categories_slug'), table_name='categories')  
    op.drop_index(op.f('ix_categories_id'), table_name='categories')  
    op.drop_table('categories')  
    # ### end Alembic commands ###
```

Рассмотрим его основные части:.
- `revision`: Это код идентификации миграции.
- `Upgrade()`: Это функция вызывается при применении изменений миграции.
- `Downgrade()`: Это функция вызывается, когда изменения миграции удаляются.

Можно использовать команду `alembic upgrade 4a7fc2a8d136⁣`, чтобы выполнить миграцию определенной ревизии. Также рассмотрим основные команды в Alembic
- `alembic upgrade +2` две версии включая текущую для апгрейда
- `alembic downgrade -1` на предыдущую для даунгрейда
- `alembic current` получить информацию о текущей версии
- `alembic history --verbose` история миграций, более подробнее можно почитать в [документации](https://alembic.sqlalchemy.org/en/latest/tutorial.html#viewing-history-ranges).
- `alembic downgrade base` даунгрейд в самое начало миграций
- `alembic upgrade head` применение самой последней созданной миграции