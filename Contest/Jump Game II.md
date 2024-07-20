---
authors:
  - Anton Petrov
status: Draft
tags:
  - contest
  - medium
  - dp
---
## Решение

## Пример решения

```Python
def jump(nums):
	n = len(nums)
	prev = [0] * n
	for i in range(1, n):
		if i <= nums[0]:
			prev[i] = prev[0] + 1
		else:
			prev[i] = prev[i - 1] + 1

	for i in range(1, n):
		current = [n] * n
		for j in range(i, n):
			if j == i:
				current[j] = prev[j]
			elif j - i <= nums[i]:
				current[j] = min(current[i] + 1, prev[j])
			else:
				current[j] = min(current[j - 1] + 1, prev[j])
		prev = current
	return prev[-1]
```

## Оптимизация

```Python
def jump(nums):
	n = len(nums)
	dp = [10**4] * n
	dp[0] = 0

	for i in range(n):
		jump = nums[i]
		j = i + 1
		while j < n and j < i + jump + 1:
			dp[j] = min(dp[j], dp[i] + 1)
			j += 1
	
	return dp[-1]
```

## Список источниковdp

- [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)