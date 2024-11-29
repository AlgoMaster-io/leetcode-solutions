# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m * n)

class Solution:
    def longestIncreasingPath(self, matrix):
        if not matrix or not matrix[0]:
            return 0

        self.directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        m, n = len(matrix), len(matrix[0])
        memo = [[0] * n for _ in range(m)]
        longest_path = 0

        def dfs(i, j):
            if memo[i][j] != 0:
                return memo[i][j]

            max_path = 1  # The cell itself can be considered as a path of length 1

            for dir in self.directions:
                x, y = i + dir[0], j + dir[1]
                if 0 <= x < m and 0 <= y < n and matrix[x][y] > matrix[i][j]:
                    length = 1 + dfs(x, y)
                    max_path = max(max_path, length)

            memo[i][j] = max_path
            return max_path

        for i in range(m):
            for j in range(n):
                longest_path = max(longest_path, dfs(i, j))

        return longest_path
```

## Approach 2: Topological Sort with Indegree

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m * n)
from collections import deque

class Solution:
    def longestIncreasingPath(self, matrix):
        if not matrix or not matrix[0]:
            return 0
        
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        m, n = len(matrix), len(matrix[0])
        indegree = [[0] * n for _ in range(m)]

        # Calculate indegrees of each cell
        for i in range(m):
            for j in range(n):
                for dir in directions:
                    x, y = i + dir[0], j + dir[1]
                    if 0 <= x < m and 0 <= y < n and matrix[x][y] > matrix[i][j]:
                        indegree[x][y] += 1

        queue = deque()
        
        # Initialize queue with cells having 0 indegree
        for i in range(m):
            for j in range(n):
                if indegree[i][j] == 0:
                    queue.append((i, j))
        
        path_length = 0
        
        # Process each cell in the queue
        while queue:
            size = len(queue)
            path_length += 1
            for _ in range(size):
                cell = queue.popleft()
                for dir in directions:
                    x, y = cell[0] + dir[0], cell[1] + dir[1]
                    if 0 <= x < m and 0 <= y < n and matrix[x][y] > matrix[cell[0]][cell[1]]:
                        indegree[x][y] -= 1
                        if indegree[x][y] == 0:
                            queue.append((x, y))

        return path_length
```

