---
authors:
  - Anton Petrov
related:
  - "[[heapq — Алгоритм очереди кучи|heapq]]"
status: Draft
tags:
  - bfs
---
## Решение

## Пример решения

```Python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k = k
        self._nums = nums
        

    def add(self, val: int) -> int:
        self._nums.append(val)
        self._nums = sorted(self._nums)
        return self._nums[-self._k] 
```

## Оптимизация

В представленном выше решении каждый раз добавляя новый элемент мы будем сортировать исходный массив заново, данное решение имеет допустимую алгоритмическую сложность по времени `O(log N)`, однако не является оптимальным поскольку сортировка будет происходить по всей длине массива при добавлении нового элемента.

Вместо этого, мы можем использовать структуру данных `heap` (кучу), которая представляет из себя дерево и в реализации на `Python` (модуль `heapq`) в начале очереди всегда хранит минимальный элемент.

Таким образом добавляя новый элемент в кучу мы можем затем извлекать минимальный элемент из кучи через метод `pop` до тех пор пока длинна массива не будет ровна `k`, и тем самым в ответе мы можем просто возвращать первый элемент из кучи, что и будет являться максимальным `k-тым` элементом.

```Python
import heapq


class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self._k, self._heap = k, nums
        heapq.heapify(self._heap)

        while len(self._heap) > k:
            heapq.heappop(self._heap)


    def add(self, val: int) -> int:
        heapq.heappush(self._heap, val)

        if len(self._heap) > self._k:
            heapq.heappop(self._heap)

        return self._heap[0]    
```

## Список источников

- [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/)