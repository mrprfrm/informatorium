---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
---

You are given an integer array `nums` sorted in **non-decreasing order**. Remove the duplicates **in-place** such that each unique element appears only once. The relative order of the elements must be kept the same.

After removal, the first `k` elements of `nums` should contain the unique numbers in sorted order, where `k` is the number of unique elements.

## Intuitive Solution

The first idea is to track already seen values using a **buffer** (for example, a `set`):

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        buf = set()
        i = j = 0
        while i < len(nums):
            if nums[i] not in buf:
                buf.add(nums[i])
                nums[j] = nums[i]
                j += 1
            i += 1
        return j
```

This works correctly but is **redundant**. Because the array is sorted, any duplicates appear **next to each other** — there is no need to store previously seen values. This solution runs in `O(n)` time but uses `O(n)` extra memory for the buffer, which is avoidable.

## Optimized Solution

Since duplicates are **contiguous**, we only need to compare each element with the previous unique one. We use two pointers:

- `i` — the **current index** being examined;
- `j` — the index of the **last unique element**.

When `nums[i]` differs from `nums[j]`, we move `j` forward and write `nums[i]` to that position.

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        j = 0
        for i in range(1, len(nums)):
            if nums[i] != nums[j]:
                j += 1
                nums[j] = nums[i]
        return j + 1
```

This approach performs a **forward compaction**, rewriting the array in place while preserving relative order. It performs a single linear pass with `O(n)` time complexity and uses constant space `O(1)` since no auxiliary structures are required.

**References**

- [26. Remove Duplicates from Sorted Array (leetcode.com)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
