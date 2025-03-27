---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - window
  - prefixsum
---

![Maximum Average Subarray I](maximum_average_subarray_i.png)

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return this value. Any answer with a calculation error less than $10^{-5}$ will be accepted.

## Intuitive Solution

According to the problem statement, we need to find the subarray of `nums` with the maximum average sum of its elements. First, let’s determine the beginning and ending indexes of such subarrays. Then, we will select the one with the largest sum, and dividing it by `k` will give us the final answer.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # Instead of using a memo array, we are going to calculate the sum on each iteration
        i = 0
        summ = -float("inf")
        for j, num in enumerate(nums):
            # If the current window size equals k,
            # evaluate the sum and move the window forward
            if j - i + 1 == k:
                summ = max(sum(nums[i: j + 1]), summ)
                i += 1
        return summ / k
```

Unfortunately, the expression `sum(nums[i: j + 1])` is inefficient within a loop, as it has a time complexity of $O(n)$, resulting in an overall time complexity of $O(n \times k)$. The space complexity is also $O(n^2)$ due to repeated slicing of the array inside the loop.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # Precompute prefix sums
        n = len(nums)
        prev = [0] * (n + 1)
        for i, num in enumerate(nums):
            prev[i + 1] = prev[i] + num

        i = 0
        summ = -float("inf")
        for j in range(1, n):
            if j - i + 1 == k:
                summ = max(prev[j] - prev[i], summ)
                i += 1
        return summ / k
```

This improves the time complexity to $O(n)$, but the space complexity remains $O(n)$ due to the prefix sum array. Let’s explore more optimal approaches.

## Optimal Solution

Let’s take a closer look at the formula used to find the maximum sum of a subarray of size `k`. Instead of manually managing window pointers on each iteration, we can directly compute the difference between the prefix sum at position `j` and the one at position `j - k`. This avoids unnecessary pointer management.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        n = len(nums)
        prev = [0] * (n + 1)
        for i, num in enumerate(nums):
            prev[i + 1] = prev[i] + num

        # No need to manage window pointers explicitly
        summ = prev[k]
        for j in range(k, n):
            summ = max(prev[j + 1] - prev[j - k + 1], summ)

        return summ / k
```

This keeps both time and space complexities at $O(n)$. However, the solution can still be optimized.

Rather than building a full prefix sum array, we can track the sum of the first `k` elements and update it in place as the window moves. This removes the need for extra space.

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        # Initialize the sum with the first k elements
        summ = prev = sum(nums[:k])
        for i in range(k, len(nums)):
            prev += nums[i] - nums[i - k]
            summ = max(prev, summ)
        return summ / k
```

This version achieves $O(n)$ time and $O(1)$ space complexity. Technically, `sum(nums[:k])` is $O(k)$, not $O(1)$, but this is a minor assumption — the impact is negligible and can be avoided with a simple loop if needed.

## References:

- [643. Maximum Average Subarray I (leetcode.com)](https://leetcode.com/problems/maximum-average-subarray-i/description/?envType=problem-list-v2&envId=m424e3ds)
