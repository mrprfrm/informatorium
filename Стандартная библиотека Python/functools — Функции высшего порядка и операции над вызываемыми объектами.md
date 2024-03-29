---
aliases:
  - functools
author:
  - Anton Petrov
last updated: 2023-09-16
related:
  - "[[Стандартная библиотека|Standard library]]"
Spell checked: true
tags:
  - python
  - caching
  - functools
  - standardlibrary
version: 0.0.2
---
## @functools.cache

Простая и легковесная кэш-функция Python, также известная как мемоизация.

Оборачивает исходную функцию, создавая тонкую обёртку вокруг `dictionary lookup` для аргументов функции для последующего доступа к соответствующим результатам выполнения функции.

> `dictionary lookup` буквально означает поиск по словарю, операцию извлечения из словаря некоторого значения по его ключу.

Поскольку данный декоратор не подразумевает удаление старых значений, он меньше меньше и быстрее, чем `lru_cache` с заданным `lim`.

```Python
@cache
def factorial(n):
    return n * factorial(n-1) if n else 1

>>> factorial(10)      # no previously cached result, makes 11 recursive calls
3628800
>>> factorial(5)       # just looks up cached value result
120
>>> factorial(12)      # makes two new recursive calls, the other 10 are cached
479001600
```

## Список источников

- [functools — Higher-order functions and operations on callable objects (python.org)](https://docs.python.org/3/library/functools.html)
- [Memoization (wikipedia.org)](https://en.wikipedia.org/wiki/Memoization)