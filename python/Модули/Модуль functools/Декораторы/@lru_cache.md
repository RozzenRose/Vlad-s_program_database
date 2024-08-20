#functools #python #LRU #декоратор


Стратегия LRU представляет собой метод управления кэшем, в котором данные, которые дольше всего не использовались, считаются наименее ценными, и они удаляются из кэша в первую очередь при необходимости освободить место для новых данных. Кэш, реализованный посредством стратегии LRU, упорядочивает элементы в порядке их использования. Каждый раз, когда мы обращаемся к записи, алгоритм LRU перемещает ее в верхнюю часть кэша. Таким образом, алгоритм может быстро определить запись, которая дольше всех не использовалась, проверив конец списка.

В модуле `functools` уже реализован декоратор `lru_cache`, дающий возможность кэшировать результат вычисления функции, используя стратегию Least Recently Used. Это просто, но мощный метод, который позволяет использовать в коде возможности кэширования.
```python
from functools import lru_cache​

@lru_cache()
def fibonacci(n):
    if n <= 2:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```
добавляет кэширование к функции `fibonacci()`. Декоратор `lru_cache` сохраняет результаты вызова `fibonacci()` для каждого уникального значения аргумента

Этот декоратор не может принимать нехэшируемые объекты.

#### Аргументы декоратора `lru_cache`
При декорировании функции с помощью декоратора `lru_cache` мы можем использовать следующие аргументы:
- `maxsize=128` — максимальный размер кэша (тип `int`)
- `typed=False` — как кэшировать при разных типах аргументов (тип `bool`)

Если для параметра `maxsize` установлено значение `None`, то кэш может расти без ограничений

Если для `type` задано значение `True`, то результаты вызовов функции для аргументов разных типов будут кэшироваться отдельно. Например, `f(3)` и `f(3.0)` будут рассматриваться как отдельные вызовы с разными результатами. Если для `typed` задано значение `False`, то вызовы будут рассматриваться как одинаковые
```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 4))
print(concat('beegeek', 5.0))
print(concat('beegeek', 4.0))
```
```
beegeek 4
beegeek 5.0
beegeek 4
```
Как мы видим, третий вызов функции с аргументами `('beegeek', 4.0)` использует закэшированный результат первого вызова с аргументами `('beegoggk', 4.0)`
```python
from functools import lru_cache

@lru_cache(typed=True)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 4))
print(concat('beegeek', 5.0))
print(concat('beegeek', 4.0))
```
```
beegeek 4
beegeek 5.0
beegeek 4.0
```

#### Дополнительные методы декоратора `lru_cache`:
Чтобы помочь измерить эффективность кэша и настроить параметр `maxsize`, декорированная функция имеет метод `cache_info()`, который возвращает именованный кортеж, показывающий `hits`, `messes`, `maxsize` и `currsize`

Мы можем использовать информацию, возвращаемую `cache_info()`, чтобы понять, как работает кэш, и настроить его, чтобы найти подходящий баланс между скоростью работы и объемом памяти:
- `hits` – количество значений, которые `lru_cache` вернул непосредственно из памяти, поскольку они присутствовали в кэше
- `misses` – количество значений, которые были вычислены, а не взяты из памяти
- `maxsize` – это размер кэша, который мы определили, передав его декоратору
- `currsize` – текущий размер кэша

```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 1))
print(concat('beegeek', 1.0))
print(concat('beegeek', True))
print(concat('beegeek', 4.0))
print(concat('beegeek', 5))

print(concat.cache_info())
```
```
beegeek 1
beegeek 1
beegeek 1
beegeek 4.0
beegeek 5
CacheInfo(hits=2, misses=3, maxsize=128, currsize=3)
```

Декоратор `lru_cache` также предоставляет метод `cache_clear()` для очистки кэша
```python
from functools import lru_cache

@lru_cache(typed=False)
def concat(text, num):
    return text + ' ' + str(num)

print(concat('beegeek', 1))
print(concat('beegeek', 1.0))
print(concat('beegeek', True))
print(concat('beegeek', 4.0))
print(concat('beegeek', 5))

print(concat.cache_info())
concat.cache_clear()
print(concat.cache_info())
```
```
beegeek 1
beegeek 1
beegeek 1
beegeek 4.0
beegeek 5
CacheInfo(hits=2, misses=3, maxsize=128, currsize=3)
CacheInfo(hits=0, misses=0, maxsize=128, currsize=0)
```