---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - window
---

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return this value. Any answer with a calculation error less than $10^{-5}$ will be accepted.

## Intuitive Solution

According to the problem statement, we need to find the subarray of `nums` with the maximum average sum of its elements. First, let's determine the beginning and ending indexes of such subarrays. Then, we will select the one with the largest sum, and dividing it by `k` will give us the final answer.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        i = j = 0
        # Instead of using a memo array, we are going to calculate the sum on each iteration
        res = -float("inf")
        while j < len(nums):
            # If the current window size exceeds k,
            # we need to shrink the window from the left side
            if j - i + 1 > k:
                i += 1

            if j - i + 1 == k:
                res = max(sum(nums[i: j + 1]), res)
                i += 1
            j += 1

        return res / k
```

Unfortunately, the expression `sum(nums[i: j + 1])` is inefficient within a loop, as it has a time complexity of $O(n)$, resulting in an overall time complexity of $O(n * k)$.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # Precompute prefix sums
        n = len(nums)
        memo = [0] * n
        memo[0] = nums[0]
        for i in range(1, n):
            memo[i] = memo[i - 1] + nums[i]

        i = j = 0
        res = -float("inf")
        while j < n:
            if j - i + 1 > k:
                i += 1

            if j - i + 1 == k:
                prev = memo[i - 1] if i > 0 else 0
                res = max(memo[j] - prev, res)
                i += 1
            j += 1

        return res / k
```

## Optimal Solution

Let's take a closer look at the formula we are using to find the maximum sum of a subarray of size `k`. In fact, there is no need to shrink the window on each iteration. Instead, we can simply compute the difference between the sum at the current position `j` and the sum at position `j - k`. This allows us to avoid unnecessary pointer management.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        n = len(nums)
        memo = [0] * n
        memo[0] = nums[0]
        for i in range(1, n):
            memo[i] = memo[i - 1] + nums[i]

        # Currently, sliding window index management is not required
        res = memo[k - 1]
        for j in range(k, n):
            res = max(memo[j] - memo[j - k], res)

        return res / k
```

Now, let's take a closer look at the first loop. Instead of calculating prefix sums for the entire `nums` array, we can compute the sum of the first `k` elements. Then, in each iteration of the second loop, we dynamically update the sum for the current window position. This eliminates the need for a separate prefix sum array, reducing both space and time complexity.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # Predefine part of the sums memo array
        # The remaining one will be calculated on the fly
        res = memo = sum(nums[:k])
        for i in range(k, len(nums)):
            memo += nums[i] - nums[i - k]
            res = max(memo, res)
        return res / k
```

## References:

- [643. Maximum Average Subarray I (leetcode.com)](https://leetcode.com/problems/maximum-average-subarray-i/description/?envType=problem-list-v2&envId=m424e3ds)
