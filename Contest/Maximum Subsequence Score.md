---
authors:
  - Anton Petrov
status: Draft
tags:
  - bfs
---
## Решение

## Пример решение

```Python
from functools import cache

def max_score(nums1, nums2, k):
	@cache
	def dfs(current, remains):
		if len(current) == k:
			s, m = 0, float("inf")
			for i in current:
				s += nums1[i]
				m = m if m < nums2[i] else nums2[i]
			yield s * m
		elif len(current) < k:
			for i, j in enumerate(remains):
				yield from dfs(current + (j,), remains[i + 1:])
	
	n = len(nums1)
	return max(dfs(tuple(), tuple(range(n))))
```

## Оптимизация

```Python
import heapq

def max_score(nums1, nums2, k):
	pairs = zip(nums1, nums2)
	pairs = sorted(pairs, key=lambda p: p[1], reverse=True)

	result, s, heap = 0, 0, []
	for a, b in pairs:
		s += a
		heapq.heappush(heap, a)

		if len(heap) > k:
			m = heapq.heappop(heap)
			s -= m
		
		if len(heap) == k:
			result = max(result, s * b)

	return result
```

## Список источников

- [2542. Maximum Subsequence Score](https://leetcode.com/problems/maximum-subsequence-score/)