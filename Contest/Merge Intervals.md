---
authors:
  - Anton Petrov
status: draft
tags:
  - medium
  - intervals
---

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## Intuitive Solution

Let's consider merging two intervals represented as ranges `[a, b]` and `[c, d]`. These two ranges can be merged if `a <= c <= b`, meaning the second interval starts within the bounds of the first — i.e., they overlap at some point.

The main challenge is that the input intervals are not initially sorted. This means that a mergeable interval could appear anywhere in the list, potentially far from its pair, which makes the merging process unnecessarily complex.

Rather than implementing partial sorting or scanning the list repeatedly during merging, we can simply **sort all intervals upfront**. By sorting the intervals based on their starting points, we ensure that overlapping intervals are adjacent, which allows us to merge them efficiently in a single pass.

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])
        res = [intervals[0]]
        for i in range(1, len(intervals)):
            l1, l2 = res[-1], intervals[i]

            if l1[0] <= l2[0] <= l1[-1]:
                # Merge the intervals
                res[-1][-1] = max(l1[-1], l2[-1])
            else:
                # No overlap, add as a new interval
                res.append(l2)
        return res
```

This solution has a time complexity of `O(n log n)` due to the initial sorting step and a space complexity of `O(n)` for storing the result array.

> We could implement a custom merge sort algorithm and merge intervals during the merge step. However, that might be overkill for this specific problem.

## References

- [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/description/?envType=problem-list-v2&envId=m424e3ds)
