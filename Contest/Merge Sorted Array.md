---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
---

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

## Intuitive Solution

According to the problem statement, we need to modify the original array `nums1` instead of returning a new list that contains values from both arrays. However, first, let's try solving the problem using an extra array.

Even with one additional array, we can't use a nested loop because such a solution would be inefficient. Instead, we loop through both lists simultaneously using appropriate pointers.

Now, the main question is: how are we going to increment the pointer values? There is no point in incrementing both pointers on each iteration, as the values in both arrays might be shuffled relative to each other. Instead, we search for the smallest value in the current iteration and increment only the pointer associated with the smallest value.

```python
m, nums1 = 3, [1, 2, 6, 0, 0, 0]
n, nums2 = 3, [2, 2, 3]

# If n is 0, there is no point in continuing
# The answer is the original nums1 array since nums2 has no elements to modify it
if n < 1:
    return

nums3 = []
i = j = 0
while m > 0 and i < m and j < n:
    if nums1[i] < nums2[j]:
        nums3.append(nums1[i])
        i += 1
    else:
        nums3.append(nums2[j])
        j += 1

# Since we can't determine which array has fewer elements,
# we need to fill nums3 with the remaining values

while j < n:
    nums3.append(nums2[j])
    j += 1

while i < m:
    nums3.append(nums1[i])
    i += 1

# According to the condition, we need to modify the original nums1 array
for i, num in enumerate(nums3):
    nums1[i] = num  # Fixed typo: "nu" -> "num"
```

## Optimized Solution

First of all, we need to pay attention to the original `nums1` array. This array has **significant** and **insignificant** parts:

- The **significant** part is the range `nums1[:m]`, which contains actual values to compare with `nums2`.
- The **insignificant** part is the range `nums1[m:]`, which is filled with zeros.

Since the insignificant part doesn’t contain meaningful data, instead of creating a new array (`nums3`), we can simply use the extra space already available in `nums1`. To do this efficiently, we traverse from the end of `nums1` to the beginning, ensuring that we don’t overwrite significant values before comparing them.

Now, we search for the largest value on each iteration, place it at the end according to the current position in `nums1`, and decrement the pointer associated with that value.

```python

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:

        # Pointer for elements in the significant part of nums1
        i = m - 1
        # Pointer for elements in nums2
        j = n - 1
        # Current position pointer
        k = m + n - 1

        # Since we are modifying nums1, there is no need to override the remaining values
        # once nums2 has been fully merged.
        while j >= 0:
            if i >= 0 and nums1[i] > nums2[j]:
                nums1[k] = nums1[i]
                i -= 1
            else:
                nums1[k] = nums2[j]
                j -= 1
            k -= 1
```

## References

- [88. Merge Sorted Array (leetcode.com)](https://leetcode.com/problems/merge-sorted-array/);
