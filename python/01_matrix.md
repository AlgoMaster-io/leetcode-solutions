# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution
```python
# Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
# Space Complexity: O(m * n), for the queue and distance matrix
from collections import deque

class Solution:
    def updateMatrix(self, mat):
        rows, cols = len(mat), len(mat[0])
        distances = [[0] * cols for _ in range(rows)]
        visited = [[False] * cols for _ in range(rows)]
        queue = deque()

        # Step 1: Initialize the queue with all '0' cells and mark them as visited
        for i in range(rows):
            for j in range(cols):
                if mat[i][j] == 0:
                    queue.append((i, j))
                    visited[i][j] = True

        # Step 2: Perform BFS to calculate distances
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        while queue:
            current = queue.popleft()
            for dir in directions:
                newRow, newCol = current[0] + dir[0], current[1] + dir[1]

                # Check bounds and if the cell is not visited
                if 0 <= newRow < rows and 0 <= newCol < cols and not visited[newRow][newCol]:
                    distances[newRow][newCol] = distances[current[0]][current[1]] + 1
                    queue.append((newRow, newCol))
                    visited[newRow][newCol] = True

        return distances
```

