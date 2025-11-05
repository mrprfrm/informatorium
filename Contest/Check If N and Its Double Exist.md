---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - hashset
  - pointers
---

You are given an integer array `arr`. Check if there exist two distinct indices `i` and `j` such that:

- `i != j`
- `0 <= i, j < len(arr)`
- `arr[i] == 2 * arr[j]`

## Intuitive Solution

A straightforward way to solve this task is to compare each pair of numbers in the array and check whether one is twice as large as the other.

```python
class Solution:
    def checkIfExist(self, arr: List[int]) -> bool:
        n = len(arr)
        for i in range(n):
            for j in range(i + 1, n):
                if arr[i] == arr[j] * 2 or arr[i] * 2 == arr[j]:
                    return True
        return False
```

This approach works but performs a full pairwise comparison, resulting in $O(n^2)$ time complexity. For small inputs, this might be acceptable, but it scales poorly as `n` grows.

## Optimized Solution

Instead of rechecking previously seen values, we can store them in a buffer and check whether the current element has its double or half already encountered.

```python
class Solution:
    def checkIfExist(self, arr: List[int]) -> bool:
        seen = set()
        for x in arr:
            if 2 * x in seen or (x % 2 == 0 and x // 2 in seen):
                return True
            seen.add(x)
        return False
```

This solution uses a **hash-set lookup**, reducing the time complexity to $O(n)$ and requiring $O(n)$ space.

## References

- [1346. Check If N and Its Double Exist (leetcode.com)](https://leetcode.com/problems/check-if-n-and-its-double-exist)
