---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
---

You are given an integer array `arr`. Replace every element with the **greatest element** among the elements to its **right**, and replace the **last element** with `-1`.

After performing the replacements, return the array.

## Solution

Instead of iterating through the array and recalculating the maximum for every element (which would require nested loops), we can reason about the structure of the problem. For any position `i`, the value we assign depends only on the elements **to its right**. Therefore, we can traverse the array **from right to left**, keeping track of the maximum value we’ve seen so far.

- Initialize a variable `buff` as the last element of the array.
- Move leftward:
  - Replace the current element with `buff`.
  - Update `buff` if the original value of the current element was greater.
- The last element is always `-1`.

```python
class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        n = len(arr)
        if n == 0:
            return arr

        buff, arr[-1] = arr[-1], -1

        for i in range(n - 2, -1, -1):
            current, arr[i] = arr[i], buff
            if current > buff:
                buff = current

        return arr
```

This method processes the array in a **single pass**, using constant extra space, with time complexity of $O(n)$ and space complexity of $O(1)$.

## References

- [1299. Replace Elements with Greatest Element on Right Side (leetcode.com)](https://leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/)
