# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Navigation
- [Approach 1: Binary Search + DFS](#approach-1-binary-search--dfs)
- [Approach 2: Binary Search + Union-Find](#approach-2-binary-search--union-find)
- [Approach 3: Dijkstra's Algorithm using Min-Heap](#approach-3-dijkstras-algorithm-using-min-heap)

### Approach 1: Binary Search + DFS

**Intuition:**

The problem involves finding the minimum time needed to traverse from the top-left to the bottom-right of a grid filled with rising water. A direct approach is to use Binary Search over time values and check feasibility using Depth First Search (DFS).

- **Binary Search:** We binary search over the possible time values, from the minimum cell value to the maximum cell value in the grid.
- **DFS check:** For each mid value in binary search, perform a DFS to see if there’s a path available from top-left to bottom-right where all the visited cell values do not exceed the mid value.

**Steps:**

1. Set the initial low (`lo`) as grid[0][0] and high (`hi`) as the maximum cell value.
2. Perform binary search to find the earliest moment you can reach from (0, 0) to (n-1, n-1).
3. For each mid value, perform DFS to explore if there's a path with all cell values ≤ mid.
4. Return the lowest valid mid.

**Code:**

```python
def swimInWater(grid):
    def canSwimToEnd(mid):
        visited = set()
        stack = [(0, 0)]
        visited.add((0, 0))
        while stack:
            x, y = stack.pop()
            if x == n - 1 and y == n - 1:
                return True
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited and grid[nx][ny] <= mid:
                    visited.add((nx, ny))
                    stack.append((nx, ny))
        return False

    n = len(grid)
    lo, hi = grid[0][0], max(max(row) for row in grid)
    while lo < hi:
        mid = (lo + hi) // 2
        if canSwimToEnd(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo
```

**Complexities:**

- **Time Complexity:** `O(n^2 log(max_val))`, where `n` is the grid's dimenstion and `max_val` is the maximum value in the grid.
- **Space Complexity:** `O(n^2)` for the visited set.

---

### Approach 2: Binary Search + Union-Find

**Intuition:**

Using the Union-Find data structure, we can incrementally flood the grid and check connectivity between the start and end cell. Union-Find helps efficiently connect components and check connectivity.

**Steps:**

1. Treat each grid cell at time `mid` as an edge. If edge exists, union the two cells.
2. Start from low (grid[0][0]) to high (max(grid[i][j])) and use this approach to check the connectivity.

**Code:**

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [1] * size
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.parent[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.parent[rootX] = rootY
            else:
                self.parent[rootY] = rootX
                self.rank[rootX] += 1

def swimInWater(grid):
    n = len(grid)
    edges = []
    for i in range(n):
        for j in range(n):
            for ni, nj in ((i + 1, j), (i, j + 1)):
                if ni < n and nj < n:
                    edges.append((max(grid[i][j], grid[ni][nj]), i * n + j, ni * n + nj))

    edges.sort()

    uf = UnionFind(n * n)
    for t, u, v in edges:
        uf.union(u, v)
        if uf.find(0) == uf.find(n * n - 1):
            return t
    return -1  # should never hit here
```

**Complexities:**

- **Time Complexity:** `O(n^2 log(n^2))` due to sorting of edges.
- **Space Complexity:** `O(n^2)` for Union-Find data structures.

---

### Approach 3: Dijkstra's Algorithm using Min-Heap

**Intuition:**

Dijkstra's algorithm, typically used for shortest path problems on graphs, can be adapted here to minimize the maximum height climbed in a path through the grid. The cost of a node is the maximum height climbed to reach that node from the start.

**Steps:**

1. Use a priority queue (min-heap) initialized with a tuple starting at (grid[0][0], 0, 0).
2. Continuously pop the least cost node, visiting neighbor nodes and updating their costs as necessary.
3. Stop when the end cell is reached and return the cost associated with reaching it.

**Code:**

```python
import heapq

def swimInWater(grid):
    n = len(grid)
    pq = [(grid[0][0], 0, 0)]
    visited = set((0, 0))
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    while pq:
        t, x, y = heapq.heappop(pq)
        if (x, y) == (n - 1, n - 1):
            return t
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < n and 0 <= ny < n and (nx, ny) not in visited:
                visited.add((nx, ny))
                heapq.heappush(pq, (max(t, grid[nx][ny]), nx, ny))
    return -1  # should never hit here

```

**Complexities:**

- **Time Complexity:** `O(n^2 log(n))` because each cell is processed and neighbor investigation uses a priority queue.
- **Space Complexity:** `O(n^2)` for the visited set and priority queue storage.

Each approach has its strengths and offers varied insight into algorithmic problem solving with grids, path finding, and optimization.

