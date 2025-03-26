---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - hashmap
---

![Intersection of Two Arrays](intersection_of_two_sets.png)

Given two integer arrays `nums1` and `nums2`, return an array of their intersection[^1]. Each element in the result must be unique and you may return the result in **any order**.

## Intuitive Solution

According to the definition of **intersection**[^1], we need to find the common values in both arrays. Additionally, according to the problem statement, these values should be **unique**.

> We don't need to maintain the original order of the values, nor do we need to consider elements between them — we are not looking for subarrays.

Now, let's determine how we can check whether a value from the `nums1` array also appears in `nums2`.

> It is sufficient to check if a value is common in only one direction.

Since the result array should contain only unique values, we can store all elements from `nums1` in a set (`memo`). Then, as we iterate through `nums2`, we check whether each value exists in the set. If it does, we remove it from the set and add it to the result array.

> We can be sure that the values added to the result are unique because of the `set` structure.

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res = []
        memo = set(nums1)
        for num in nums2:
            if num in memo:
                res.append(num)
                memo.remove(num)
        return res
```

The solution has `O(M + N)` time complexity, where `M` and `N` are the sizes of the input arrays, and space complexity is `O(N)`. Here, we loop over one of the arrays and use an additional set to store the values from the other array. The solution is pretty solid in a common-sense way, but Python has a native `set` data type that supports all the necessary operations, which might be extremely useful in particular cases—so let's dive into that.

## Set Intersection Solution

According to the problem statement, we can simply use Python's built-in `set` structure and its intersection operator.

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

We might say that now the solution has only `O(1)` time and space complexity, but that's not actually true. Under the hood, casting a `list` to a `set` and vice versa requires looping through the entire collection, and its size has a direct effect on both the runtime and memory usage.

So, the actual time and space complexity is about `O(M + N)`. The good news is that these operations are implemented in highly optimized C code within CPython, making them significantly faster than equivalent logic written purely in Python. 

## References

- [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays)
- [Built-in Types (python.org)](https://docs.python.org/3/library/stdtypes.html)

[^1]: The intersection of two arrays is defined as the set of elements that are present in both arrays.
