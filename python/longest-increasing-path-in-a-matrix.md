## [Problem: Leetcode 329 - Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

**Approaches:**

1. [DFS with Memoization](#dfs-with-memoization)
2. [Topological Sort](#topological-sort)

---

### Approach 1: DFS with Memoization

#### Intuition:

The problem requires us to find the longest increasing path in a 2D matrix. A brute-force approach would involve trying every possible path from every cell, leading to a time complexity that would be too high. To optimize, we can use Depth-First Search (DFS) with memoization, treating each cell as a node in a graph where an edge exists to an adjacent node if the value is greater.

#### Detailed Explanation:

- **DFS**:
  - For each cell in the matrix, start a DFS. The path continues if the next cell has a greater value.
  - Each cell can be explored in four directions: up, down, left, and right.
  
- **Memoization**:
  - Use a memo table where `memo[i][j]` stores the length of the longest increasing path starting at cell `(i, j)`.
  - If a cell's longest path length is already computed, return it instead of recomputing.

- **Recursive Function**:
  - Define a recursive function `dfs(x, y)` that returns the length of the longest path starting from `(x, y)` using the memo table to avoid recalculations.

#### Time and Space Complexity:

- **Time Complexity**: O(m * n), where m is the number of rows and n is the number of columns. Each cell is computed once.
- **Space Complexity**: O(m * n) due to the recursion stack and memoization table.

#### Python Code:

```python
def longestIncreasingPath(matrix):
    if not matrix or not matrix[0]:
        return 0

    m, n = len(matrix), len(matrix[0])
    memo = [[-1] * n for _ in range(m)]

    def dfs(x, y):
        if memo[x][y] != -1:
            return memo[x][y]
        
        max_len = 1
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]  # up, down, left, right

        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n and matrix[nx][ny] > matrix[x][y]:
                max_len = max(max_len, 1 + dfs(nx, ny))  # Process neighboring cell
        
        memo[x][y] = max_len  # Store result in memo table
        return max_len

    max_path = 0
    for i in range(m):
        for j in range(n):
            max_path = max(max_path, dfs(i, j))  # Evaluate longest path from each cell

    return max_path
```

---

### Approach 2: Topological Sort

#### Intuition:

Another approach is to use a topological sort, which is more efficient in practice for sparse graphs. It is based on the graph theoretical insight that the longest path in a Directed Acyclic Graph (DAG) can be determined by topological order processing.

#### Detailed Explanation:

- **Graph Representation**:
  - Consider each cell as a node. There is a directed edge from cell `(i, j)` to `(k, l)` if `matrix[k][l] > matrix[i][j]`.
  
- **Calculate Indegree**:
  - First, compute the indegree for each vertex. The indegree of a node denotes how many nodes have an edge directed towards it.

- **Topological Sort via Kahn's Algorithm**:
  - Initialize a queue and enqueue all nodes with indegree 0.
  - Process each node by removing it from the queue, then reduce the indegree for its neighbors, and enqueue any neighbor that reaches 0 indegree.
  
- **Determine Path Length**:
  - As we process nodes, we determine the path length by counting layers of nodes processed.

#### Time and Space Complexity:

- **Time Complexity**: O(m * n), due to processing each cell and its neighbors.
- **Space Complexity**: O(m * n), for storing the indegree and the queue.

#### Python Code:

```python
from collections import deque

def longestIncreasingPath(matrix):
    if not matrix or not matrix[0]:
        return 0
    
    m, n = len(matrix), len(matrix[0])
    indegree = [[0] * n for _ in range(m)]
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    # Calculate indegrees
    for i in range(m):
        for j in range(n):
            for dx, dy in directions:
                ni, nj = i + dx, j + dy
                if 0 <= ni < m and 0 <= nj < n and matrix[ni][nj] > matrix[i][j]:
                    indegree[ni][nj] += 1
    
    # Perform topological sort
    queue = deque()
    for i in range(m):
        for j in range(n):
            if indegree[i][j] == 0:
                queue.append((i, j))
    
    length = 0
    while queue:
        length += 1
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and matrix[nx][ny] > matrix[x][y]:
                    indegree[nx][ny] -= 1
                    if indegree[nx][ny] == 0:
                        queue.append((nx, ny))
    
    return length
```

Both approaches solve the problem efficiently, with the DFS with memoization being slightly easier to implement and understand, while the topological sort provides a more algorithmically intriguing method for finding the longest path.

