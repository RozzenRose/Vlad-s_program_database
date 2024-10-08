#модуль #functools #python 


![[Pasted image 20240621153004.png]]
Модуль содержит обширный набор инструментов для работы с функциями. Функции высшего порядка, которые можно использовать для кэширования , перегрузки, создания декораторов и в целом для того, чтобы делать код более фукнциональным.

Подключение:
```python
import functools
from functools import*
```

Методы:
- [[reduce()]] 
- [[cmp_to_key()]] - сортировка
- [[partial()]] - частичное выполнение функций
- [[update_wrapper()]] - переносит данные из функции в `partial` объект на ее основе

Декораторы:
 - [[@functools.wrapt]] - декорирование с сохранением оригинальных атрибутов
 - [[@lru_cache]] - декоратор с функционалом кэширования, реализованным в соответствии со стратегией LRU
 - [[python/Модули/Модуль functools/Декораторы/@singledispatchmethod|@singledispatchmethod]] - позволяет переопределять функции в классе в зависимости от того, какие данные подаются на вход
 - [[python/Модули/Модуль functools/Декораторы/@total_ordering|@total_ordering]] - позволяет создать полный функционал для компарирования внутри класса на основе всего двух функций сравнения для экземпляра класса

Чистые функции — это строительные блоки в функциональном программировании. Они обладают двумя ключевыми свойствами:
1. **Детерминированность**: Чистая функция всегда возвращает один и тот же результат для одних и тех же значений аргументов. Нет случайных или неопределенных вариантов.
2. **Отсутствие побочных эффектов**: Выполнение чистой функции не изменяет состояние за пределами её области видимости и не оказывает видимого воздействия на внешний мир, кроме возвращения значения. Никаких побочных эффектов, таких как изменение глобальных переменных или вывод на экран, не происходит

Документации по модулю `functools` доступна по [ссылке](https://docs.python.org/3/library/functools.html).
Исходный код модуля `functools` доступен по [ссылке](https://github.com/python/cpython/blob/7a4791e03613bfbdc0d3ddfabfc0b59e6a6f7358/Lib/functools.py).
Подробнее про алгоритмы кэширования можно почитать по [ссылке](https://ru.wikipedia.org/wiki/%D0%90%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC%D1%8B_%D0%BA%D1%8D%D1%88%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F).