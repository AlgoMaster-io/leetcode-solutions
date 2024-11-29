# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
python
```python
# Import necessary libraries
import heapq

# Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
# Space Complexity: O(m * n)

class Solution:
    # Direction vectors for exploring adjacent cells (right, left, down, up)
    DIRECTIONS = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    def minimumEffortPath(self, heights):
        m = len(heights)
        n = len(heights[0])

        # Min-heap priority queue to select the path with the minimum effort so far
        min_heap = [(0, 0, 0)]  # (effort, x, y)

        # Efforts matrix to track the minimum effort required to reach each cell
        efforts = [[float('inf')] * n for _ in range(m)]
        efforts[0][0] = 0

        while min_heap:
            # Extract the cell with the minimum effort
            current_effort, x, y = heapq.heappop(min_heap)

            # If we've reached the bottom-right corner
            if x == m - 1 and y == n - 1:
                return current_effort

            # Explore all adjacent cells
            for dx, dy in self.DIRECTIONS:
                new_x, new_y = x + dx, y + dy

                # Check bounds
                if 0 <= new_x < m and 0 <= new_y < n:
                    # Calculate the effort required to move to the adjacent cell
                    effort = max(current_effort, abs(heights[new_x][new_y] - heights[x][y]))

                    # If this path to the adjacent cell is better, update efforts and heap
                    if effort < efforts[new_x][new_y]:
                        efforts[new_x][new_y] = effort
                        heapq.heappush(min_heap, (effort, new_x, new_y))

        return 0  # Shouldn't reach here. Always possible to reach the end.
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
python
```python
# Import necessary libraries
from collections import deque

# Time Complexity: O(m * n * log(maxDifference))
# Space Complexity: O(m * n)

class Solution:
    # Direction vectors for exploring adjacent cells (right, left, down, up)
    DIRECTIONS = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    
    def minimumEffortPath(self, heights):
        m = len(heights)
        n = len(heights[0])
        left, right = 0, 1_000_000
        result = 1_000_000

        while left <= right:
            mid = left + (right - left) // 2
            if self.canReachEndWithEffort(heights, mid, m, n):
                result = mid
                right = mid - 1
            else:
                left = mid + 1

        return result

    # Helper function to determine if there is a path from (0, 0) to (m - 1, n - 1) under the given maxEffort
    def canReachEndWithEffort(self, heights, maxEffort, m, n):
        visited = [[False] * n for _ in range(m)]
        queue = deque([(0, 0)])
        visited[0][0] = True

        while queue:
            x, y = queue.popleft()

            if x == m - 1 and y == n - 1:
                return True

            for dx, dy in self.DIRECTIONS:
                new_x, new_y = x + dx, y + dy

                if 0 <= new_x < m and 0 <= new_y < n and not visited[new_x][new_y]:
                    current_effort = abs(heights[new_x][new_y] - heights[x][y])

                    if current_effort <= maxEffort:
                        visited[new_x][new_y] = True
                        queue.append((new_x, new_y))

        return False
```

