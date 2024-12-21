## [Leetcode Problem 994: Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

Approaches:
- [Approach 1: Breadth-First Search (BFS) with Queue](#approach-1-bfs-with-queue)
- [Approach 2: Optimized BFS](#approach-2-optimized-bfs)

### Approach 1: Breadth-First Search (BFS) with Queue

**Intuition:**

This problem can be visualized as a graph problem where each orange in the grid is a node. Rotten oranges spread rot to their neighboring fresh oranges, similar to a BFS traversal in which each rotten orange spreads to its adjacent nodes.

The goal is to apply a multi-source BFS (starting BFS from all initially rotten oranges) and determine how many minutes it takes for all fresh oranges to rot, if possible.

#### Steps:
1. Initialize a queue and enqueue all initially rotten oranges along with a timestamp (minute 0).
2. Count the initial number of fresh oranges.
3. Process the queue:
   - For each rotten orange, attempt to rot each of its adjacent fresh oranges by checking the four possible neighboring directions.
   - If an adjacent orange is fresh, rot it, decrement the count of fresh oranges, and enqueue it with the incremented timestamp.
4. If all fresh oranges have rotted, return the timestamp of the last enqueued orange (time taken).
5. If any fresh oranges remain, return -1.

```python
from collections import deque

def orangesRotting(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_oranges = 0
    
    # Initialize the queue with all the initially rotten oranges
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))  # Include timestamp 0
            elif grid[r][c] == 1:
                fresh_oranges += 1
    
    # Directions for neighboring cells in the grid
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    minutes_elapsed = 0
    
    # Perform BFS
    while queue:
        r, c, minutes_elapsed = queue.popleft()
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc  # Calculate neighbor's position
            # Check if the neighbor is a fresh orange
            if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                grid[nr][nc] = 2  # Rot the orange
                fresh_oranges -= 1
                queue.append((nr, nc, minutes_elapsed + 1))
    
    return minutes_elapsed if fresh_oranges == 0 else -1

# Time Complexity: O(N), where N is the number of cells in the grid.
# Space Complexity: O(N), for the queue that can grow up to the size of the grid.
```

### Approach 2: Optimized BFS

**Intuition:**

The initial approach is already a BFS, and quite optimal for this problem's requirements. However, optimizations can focus on limiting the operations or improving clarity, such as reducing the number of checks within the loop or precomputing some states.

In this particular problem, since BFS handles the grid size efficiently even when fully filled, any major optimizations over the BFS are limited. However, clarity and maintenance can be improved by well-organized code.

```python
def orangesRottingOptimized(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_oranges = 0
    
    # Initialization
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_oranges += 1
    
    # Directions array for moving in the grid
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    minutes_elapsed = 0
    
    # Execute BFS level by level
    while queue and fresh_oranges > 0:
        for _ in range(len(queue)):
            x, y = queue.popleft()
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < rows and 0 <= ny < cols and grid[nx][ny] == 1:
                    grid[nx][ny] = 2
                    fresh_oranges -= 1
                    queue.append((nx, ny))
        
        # Increase time after processing each level
        minutes_elapsed += 1
    
    return minutes_elapsed if fresh_oranges == 0 else -1

# Time Complexity: O(N), where N is the number of cells in the grid.
# Space Complexity: O(N), since we might rot all oranges which get stored in the queue.
```

Both implementations effectively solve the problem using BFS. The second approach is structured for clarity by grouping queue processing and checks, which can be beneficial in more complex problem setups.

