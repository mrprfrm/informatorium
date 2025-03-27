---
authors:
  - Anton Petrov
status: note
tags:
  - medium
  - math
---

![Count Total Number of Colored Cells](count_total_number_of_colored_cells.png)

There exists an infinitely large two-dimensional grid of uncolored unit cells. You are given a positive integer `n`, indicating that you must do the following routine for `n` minutes:

- At the first minute, color **any** arbitrary unit cell blue.
- Every minute thereafter, color blue **every** uncolored cell that touches a blue cell.

Below is a pictorial representation of the state of the grid after minutes 1, 2, and 3.

Return the number of **colored cells** at the end of `n` minutes.

## Intuitive Solution

First, let's simulate the process of coloring the cells in the grid. We'll define an array that keeps track of the number of cells colored at each minute. At each step, we only need to color one new layer above the previous one. The new layer consists of `2 \times (i + 1)` cells from two sides and `2 \times (i - 1)` cells from the other two sides, where `i` is the current minute index.

```python
class Solution:
    def coloredCells(self, n: int) -> int:
        # In the first minute, we color one cell.
        # It doesn't matter how we initialize the remaining cells,
        # since we explicitly update their values at each step based on the previous ones.
        memo = [1] * n
        for i in range(1, n):
            memo[i] = memo[i - 1] + 2 * (i + 1) + 2 * (i - 1)
        return memo[-1]
```

Now, let's optimize the solution. In fact, we don't need to store the entire array of colored cells—we can just keep track of the last computed result and update it at each step. Also, let's simplify the formula:

```python
class Solution:
    def coloredCells(self, n: int) -> int:
        count = 1
        for i in range(1, n):
            # Expanding the formula: 2 * (i + 1) + 2 * (i - 1)
            # 2 * i + 2 + 2 * i - 2 = 4 * i
            count += 4 * i
        return count
```

This gives us a solution with $O(n)$ time complexity and $O(1)$ space complexity, which is quite solid—but not the most efficient. If we carefully analyze the formula, we can see that it simplifies to:

```python
class Solution:
    def coloredCells(self, n: int) -> int:
        return 1 + 2 * n * (n - 1)
```

This final version achieves $O(1)$ time complexity while maintaining correctness.

## References

- [2579. Count Total Number of Colored Cells (leetcode.com)](https://leetcode.com/problems/count-total-number-of-colored-cells)
