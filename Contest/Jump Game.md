---
authors:
  - Anton Petrov
status: Draft
tags:
  - dp
---
## Решение

## Пример решения

```Python
def canJump(nums):
	n = len(nums)
	if n == 1:
		return True

	dp = [0] * n
	dp[0] = nums[0]

	for i in range(1, n):
		if i <= dp[i - 1]:
			dp[i] = max(i + nums[i], dp[i - 1])
		
		if dp[i] >= n - 1:
			return True

	return False
```