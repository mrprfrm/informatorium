---
authors:
  - Anton Petrov
status: Draft
tags:
  - contest
  - hard
  - recursion
---
## Пример решения

```Python
from functools import cache

def longest_obstacle_course(obstacles):
	@cache
	def dfs(seq, prev):
		m = len(seq)
		
		if m == 0:
			return 0

		max_count = 0
		for j in range(m):
			current = seq[m - j - 1]
			if current <= prev:
				count = dfs(seq[:m - j - 1], current) + 1
				if count > max_count:
					max_count = count
				
		return max_count

	n = len(obstacles)

	counts = [0] * n
	tpl = tuple(obstacles)
	for i in range(0, n):
		counts[i] = dfs(tpl[:i + 1], tpl[i])
	
	return counts
```

## Список источников

- [1964. Find the Longest Valid Obstacle Course at Each Position](https://leetcode.com/problems/find-the-longest-valid-obstacle-course-at-each-position/)