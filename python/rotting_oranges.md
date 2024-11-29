# 994. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Solution
python
```python
# Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
# Space Complexity: O(m * n), for the queue and visited cells
from collections import deque

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        queue = deque()
        fresh_count = 0

        # Step 1: Initialize the queue with all rotten oranges and count fresh oranges
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 2:
                    queue.append((i, j))
                elif grid[i][j] == 1:
                    fresh_count += 1

        # Step 2: Perform BFS to rot adjacent fresh oranges
        minutes = 0
        directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

        while queue and fresh_count > 0:
            for _ in range(len(queue)):
                current = queue.popleft()
                for dir in directions:
                    new_row, new_col = current[0] + dir[0], current[1] + dir[1]

                    # Check bounds and if the orange is fresh
                    if 0 <= new_row < rows and 0 <= new_col < cols and grid[new_row][new_col] == 1:
                        grid[new_row][new_col] = 2  # Rot the orange
                        queue.append((new_row, new_col))
                        fresh_count -= 1
            minutes += 1

        # If there are still fresh oranges left, return -1
        return minutes if fresh_count == 0 else -1
```

