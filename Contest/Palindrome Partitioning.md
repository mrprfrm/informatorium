---
tags:
 - contest
 - medium
 - recursion
 - palindrome
---

![[palindrome_partitioning.png]]

## Пример решения

```Python
from functools import cache

@cache
def partition(self, s: str) -> List[List[str]]:
	if not s:
		return [[]]
	result, n = [], len(s)
	for i in range(1, n + 1):
		if s[:i] == s[:i][::-1]:
			for sub in self.partition(s[i:]):
				result.append([s[:i]] + sub)
	return result
```

## Список источников

- [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)