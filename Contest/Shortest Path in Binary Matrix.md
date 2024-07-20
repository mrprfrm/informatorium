---
authors:
  - Anton Petrov
status: Draft
tags:
  - contest
  - medium
  - bfs
---
![[shoortest_path_in_binary_matrix.png]]

## Пример решения

```Python
def shortest_path_binary_matrix(grid):
        if grid[0][0] != 0 or grid[-1][-1] != 0:
            return -1

        n = len(grid)
        queue = deque([(1, 0, 0)])
        visited = set([(0, 0)])
        while queue:
            count, i, j = queue.popleft()
            visited.add((i, j))
            if i == n - 1 and j == n -1:
                return count

            neighbours = [
                (i - 1, j - 1), (i - 1, j), (i - 1, j + 1),
                (i, j - 1), (i, j + 1),
                (i + 1, j - 1), (i + 1, j), (i + 1, j + 1),
            ]
            for i1, j1 in neighbours:
                if (
                    0 <= i1 < n and
                    0 <= j1 < n and
                    grid[i1][j1] == 0 and
                    (i1, j1) not in visited
                ):
                    queue.append((count + 1, i1, j1))
                    visited.add((i1, j1))

        return -1
```

## Список источников

- [1091. Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/)