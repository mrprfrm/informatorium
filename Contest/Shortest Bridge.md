---
authors:
  - Anton Petrov
status: Draft
tags:
  - contest
  - medium
  - bfs
---
![[shortest_bridge.png]]

## Пример решения

```python
def shortest_bridge(grid):
        n = len(grid)
        deltas = [(1, 0), (0, -1), (0, 1), (-1, 0)]
        
        island_queue = deque()
        water_queue = deque()
        visited = set()

        def bfs(queue, fnc):
            while queue:
                count, i, j = queue.popleft()
                for di, dj in deltas:
                    i1, j1 = i + di, j + dj
                    if (
                        0 <= i1 < n and
                        0 <= j1 < n and
                        (i1, j1) not in visited
                    ):
                        visited.add((i1, j1))
                        if result := fnc(count, i1, j1):
                            return result

        def handle_island_search(count, i, j):
            if grid[i][j] == 1:
                island_queue.append((None, i, j))
            else:
                water_queue.append((1, i, j))

        def handle_water_search(count, i, j):
            if grid[i][j] == 1:
                return count
            else:
                water_queue.append((count + 1, i, j))

        i, j = 0, 0
        while grid[i][j] != 1:
            i = i if j < n - 1 else i + 1
            j = j + 1 if j < n - 1 else 0
        island_queue.append((None, i, j))
        visited.add((i, j))
        
        bfs(island_queue, handle_island_search)
        return bfs(water_queue, handle_water_search)
```

## Список источников

- [934. Shortest Bridge](https://leetcode.com/problems/shortest-bridge/description/)