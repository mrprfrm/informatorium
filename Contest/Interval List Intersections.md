---
authors:
  - Anton Petrov
status: draft
tags:
  - medium
  - pointers
---

You are given two lists of closed intervals, `firstList` and `secondList`, where `firstList[i] = [starti, endi]` and `secondList[j] = [startj, endj]`. Each list of intervals is pairwise **disjoint** and in **sorted order**.

Return the intersection of these two interval lists.

A **closed interval** `[a, b]` (with `a <= b`) denotes the set of real numbers `x` with `a <= x <= b`.

The **intersection** of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of `[1, 3]` and `[2, 4]` is `[2, 3]`.

## Intuitive Solution

According to the problem statement, since both interval lists are sorted, we can iterate through them in parallel and compare the current interval from the first list with the current interval from the second one at each step. This approach guarantees that we won't miss any intersections and still maintains linear time complexity. Let's consider an example:

```python
firstList = [[0, 2], [5, 10]]
secondList = [[1, 5], [8, 12], [15, 24]]

# Let's start with `i` and `j` both set to 0

l1 = firstList[0]  # [0, 2]
l2 = secondList[0] # [1, 5]

# Here we can see that the intervals have an intersection: [1, 2]
# [0, 1, 2]
#  0 [1, 2, 3, 4, 5]

# We can determine that the intersection of these two intervals is between:
# max(0, 1) and min(2, 5) => [1, 2]
intersection = [max(l1[0], l2[0]), min(l1[1], l2[1])]  # [1, 2]

# Since the end of the second interval is greater than the end of the first one,
# there is a chance that the next interval from the first list might also intersect
# with the current interval from the second list.

# According to this logic, we move only pointer `i` (associated with the first list)
# and keep pointer `j` as it is.

l1 = firstList[1]  # [5, 10]
l2 = secondList[0] # [1, 5]
intersection = [max(l1[0], l2[0]), min(l1[1], l2[1])]  # [5, 5]

# This time, `i` remains unchanged and we move `j` to the next interval
# from the second list and repeat the same steps until the end.

l1 = firstList[1]  # [5, 10]
l2 = secondList[1] # [8, 12]
intersection = [max(l1[0], l2[0]), min(l1[1], l2[1])]  # [8, 10]

# And so on...
l1 = firstList[1]  # [5, 10]
l2 = secondList[2] # [15, 24]
intersection = [max(l1[0], l2[0]), min(l1[1], l2[1])]  # []
```

Now let's proceed with the implementation:

```python
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        i = j = 0
        res = []
        while i < len(firstList) and j < len(secondList):
            l1, l2 = firstList[i], secondList[j]
            left = max(l1[0], l2[0])
            right = min(l1[-1], l2[-1])

            if left <= right:
                res.append([left, right])

            if l1[-1] < l2[-1]:
                i += 1
            else:
                j += 1

        return res
```

The time complexity of this solution is `O(N + M)`, where `N` and `M` are the lengths of the first and second lists, respectively. The space complexity is `O(K)`, where `K` is the length of the resulting list of intersections.

## References

- [986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/description/?envType=problem-list-v2&envId=m17t1ade)
