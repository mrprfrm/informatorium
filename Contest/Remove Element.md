---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
---

You are given an integer array `nums` and an integer `val`. You need to **remove all occurrences** of `val` from the array _in-place_. The order of the remaining elements may be changed. Finally, return the number of elements that are not equal to `val`.

## Intuitive Solution

The simplest way to approach this task is to iterate through the array and remove any element equal to `val`. In Python, the most straightforward approach is to use the `pop()` method.

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        while i < len(nums):
            if nums[i] == val:
                nums.pop(i)
            else:
                i += 1
        return len(nums)
```

This implementation works correctly but is **inefficient**. Each `pop(i)` call shifts all the following elements one position to the left, resulting in $O(n^2)$ time complexity in the worst case. Although simple, this approach quickly becomes suboptimal for large arrays.

## Forward-Compact Solution

To optimize the solution, we can avoid `pop()` and instead **compact valid elements forward**. We use two pointers:

- `i`: iterates through all elements (reader)
- `k`: tracks where the next valid element should be placed (writer)

Whenever we find an element not equal to `val`, we copy it to position `k` and increment `k`. This preserves the relative order of elements and avoids expensive shifts.

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        k = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[k] = nums[i]
                k += 1
        return k
```

It gives us $O(n)$ time complexity — single pass over the array, $O(1)$ space complexity — fully in-place and preserved elements order. The remaining elements beyond index `k` are irrelevant.

## Reverse-Swap Solution

The problem statement explicitly allows **changing the order** of elements. When order doesn’t matter, we can optimize even further by applying the **reverse-swap** technique.

The idea is simple:

- Keep two boundaries:
  `i` — current index (left boundary),
  `n` — end of the active region (right boundary).
- If the current element equals `val`, replace it with the last unprocessed element (`nums[n - 1]`) and shrink the range (`n -= 1`).
- Otherwise, move `i` forward.

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        n = len(nums)
        while i < n:
            if nums[i] == val:
                nums[i] = nums[n - 1]
                n -= 1
            else:
                i += 1
        return n
```

The key observation is that **we don’t increment `i` after a swap**, because the new element brought from the end might also equal `val`. We keep checking until we confirm that the current position holds a valid element.

This approach runs in $O(n)$ time, uses $O(1)$ extra space and can reduce unnecessary writes when many elements are removed.

## References

- [27. Remove Element (leetcode.com)](https://leetcode.com/problems/remove-element/)
