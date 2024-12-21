# [Leetcode Problem 200: Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Solutions:

- [Approach 1: Depth-First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union Find (Disjoint Set)](#approach-3-union-find-disjoint-set)

---

## Approach 1: Depth-First Search (DFS)

### Intuition:
The grid represents water (`'0'`) and land (`'1'`). To count the number of islands, we need to count the distinct connected components of `'1'`. We can start from any `'1'`, perform a DFS to mark all connected lands, and increase our island count. This way, each DFS traversal marks all nodes of one island.

### Detailed Steps:
1. Iterate over each cell in the grid.
2. When a land cell (`'1'`) is found, increment the island count.
3. Perform DFS starting from this cell to mark all parts of the island.
4. Mark visited land parts as water (`'0'`) to avoid revisiting.
5. Continue until all cells are processed.

```python
def numIslands(grid):
    if not grid:
        return 0
    
    def dfs(r, c):
        if r < 0 or c < 0 or r >= len(grid) or c >= len(grid[0]) or grid[r][c] == '0':
            return
        grid[r][c] = '0'  # Mark the land as visited
        # Explore the neighboring cells in 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)

    island_count = 0
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                island_count += 1
                dfs(r, c)  # Start a DFS for this island

    return island_count
```

- **Time Complexity:** \(O(M \times N)\), where \(M\) is the number of rows and \(N\) is the number of columns, as each cell is visited once.
- **Space Complexity:** \(O(M \times N)\) in the worst case for the recursive DFS stack.

---

## Approach 2: Breadth-First Search (BFS)

### Intuition:
Similar to DFS, we can use BFS to traverse each island. Starting from any unvisited land, we use a queue to explore each point and its neighboring `'1'`s and mark them as visited.

### Detailed Steps:
1. Use a queue to perform the BFS.
2. When a `'1'` is encountered, mark it as visited and explore all connected `'1'`s using a queue.
3. Perform BFS until all connected land is visited.

```python
from collections import deque

def numIslands(grid):
    if not grid:
        return 0
    
    def bfs(r, c):
        queue = deque()
        queue.append((r, c))
        grid[r][c] = '0'
        while queue:
            row, col = queue.popleft()
            for x, y in [(row + 1, col), (row - 1, col), (row, col + 1), (row, col - 1)]:
                if 0 <= x < len(grid) and 0 <= y < len(grid[0]) and grid[x][y] == '1':
                    grid[x][y] = '0'
                    queue.append((x, y))

    island_count = 0
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                island_count += 1
                bfs(r, c)  # Start a BFS for this island

    return island_count
```

- **Time Complexity:** \(O(M \times N)\), same as DFS.
- **Space Complexity:** \(O(\min(M, N))\), for BFS queue space in the most restrictive dimension.

---

## Approach 3: Union Find (Disjoint Set)

### Intuition:
Union-Find is a data structure that keeps track of a partition of a set into disjoint subsets. We can view each land cell (`'1'`) as a node and connect nodes within the same island. The number of unique parents represents the number of islands.

### Detailed Steps:
1. Initialize a Union-Find structure with all land nodes.
2. For each land cell, unite it with its adjacent land cells (down and right to avoid duplicates).
3. The number of disjoint sets after processing gives the number of islands.

```python
class UnionFind:
    def __init__(self, grid):
        self.parent = {}
        self.rank = {}
        self.count = 0
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == '1':
                    self.parent[(r, c)] = (r, c)
                    self.rank[(r, c)] = 0
                    self.count += 1

    def find(self, p):
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)
        if rootP != rootQ:
            if self.rank[rootP] > self.rank[rootQ]:
                self.parent[rootQ] = rootP
            elif self.rank[rootP] < self.rank[rootQ]:
                self.parent[rootP] = rootQ
            else:
                self.parent[rootQ] = rootP
                self.rank[rootP] += 1
            self.count -= 1

def numIslands(grid):
    if not grid or not grid[0]:
        return 0
    
    uf = UnionFind(grid)
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                # Check and union with the right cell
                if c < len(grid[0]) - 1 and grid[r][c + 1] == '1':
                    uf.union((r, c), (r, c + 1))
                # Check and union with the down cell
                if r < len(grid) - 1 and grid[r + 1][c] == '1':
                    uf.union((r, c), (r + 1, c))
                    
    return uf.count
```

- **Time Complexity:** \(O(M \times N \cdot \alpha(MN))\), where \(\alpha\) is the inverse Ackermann function, practically constant.
- **Space Complexity:** \(O(M \times N)\), for the Union-Find data structure.

By choosing among DFS, BFS, or Union-Find based on requirements, you can efficiently solve the problem of counting the number of islands in various scenarios.

