# [Leetcode Problem 542: 01 Matrix](https://leetcode.com/problems/01-matrix/)

This problem requires determining the distance of each cell in a matrix from the nearest 0 cell. We will explore various approaches to solve it.

## Approaches
- [Approach 1: Brute Force BFS from each "1"](##approach-1:-brute-force-bfs-from-each-"1")
- [Approach 2: Multi-source BFS from all "0"s](##approach-2:-multi-source-bfs-from-all-"0"s)
- [Approach 3: Dynamic Programming](##approach-3:-dynamic-programming)

---

## Approach 1: Brute Force BFS from each "1"

### Intuition
In this basic approach, for each "1" in the matrix, initiate a BFS search to find the nearest "0". This naive method guarantees finding the shortest distance for each cell, but it executes a BFS for every single "1" in the matrix, leading to high time complexity.

### Steps:
1. Traverse each cell in the matrix.
2. For each "1", perform a BFS to locate the nearest "0" and note down the distance.
3. Use a queue to maintain BFS state, and a set to keep track of visited cells to prevent revisits.
4. For each "0" found, update the distance for that "1" in the matrix and proceed to the next cell.

### Code
```python
from collections import deque

def updateMatrix(matrix):
    # Dimensions of the matrix
    rows, cols = len(matrix), len(matrix[0])
    # Directions array for exploring neighbors (up, down, left, right)
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    # Function to perform BFS from a given starting point
    def bfs_from_point(start):
        queue = deque([start])  # Initialize queue with the starting point
        visited = set(queue)    # A set to keep track of visited points
        distance = 0
        
        while queue:
            # Explore the current level in BFS; level signifies distance
            for _ in range(len(queue)):
                x, y = queue.popleft()
                if matrix[x][y] == 0:
                    return distance
                # Check all four possible directions
                for dx, dy in directions:
                    nx, ny = x + dx, y + dy
                    if 0 <= nx < rows and 0 <= ny < cols and (nx, ny) not in visited:
                        visited.add((nx, ny))
                        queue.append((nx, ny))
            distance += 1
    
    # Initialize result matrix
    result = [[0] * cols for _ in range(rows)]
    
    # Traverse matrix to perform BFS for each "1"
    for r in range(rows):
        for c in range(cols):
            if matrix[r][c] == 1:
                # Find nearest "0" for this "1"
                result[r][c] = bfs_from_point((r, c))
    
    return result
```

### Complexity
- **Time Complexity:** O((m * n) * (m + n)), where m and n are the dimensions of the matrix. Worst-case scenario does a full grid traversal `(m*n)` for each cell `(m+n)`.
- **Space Complexity:** O(m * n), for the `visited` set and result matrix.

---

## Approach 2: Multi-source BFS from all "0"s

### Intuition
Instead of performing BFS from each "1", begin BFS from all "0"s simultaneously. Leverage a queue initialized with positions of all initial "0"s, radiating outward to compute minimum distances to "1"s.

### Steps:
1. Create a queue containing all initial positions of "0"s to act as BFS start points.
2. Initialize a result matrix, setting each "1" cell temporarily as infinity (to signify unvisited).
3. Perform BFS while updating distances to zero for each neighbor, marking cells as visited.
4. Record minimum distances required to convert neighbors to zero.

### Code
```python
from collections import deque

def updateMatrix(matrix):
    rows, cols = len(matrix), len(matrix[0])
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    queue = deque()
    # Initialize result matrix
    result = [[float('inf')] * cols for _ in range(rows)]
    
    # Enqueue all 0 positions in the matrix
    for r in range(rows):
        for c in range(cols):
            if matrix[r][c] == 0:
                queue.append((r, c))
                result[r][c] = 0
    
    # BFS from all 0s simultaneously
    while queue:
        x, y = queue.popleft()
        # Check all four neighbors
        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            # Validate position and check if this is a better solution
            if 0 <= nx < rows and 0 <= ny < cols and result[nx][ny] > result[x][y] + 1:
                result[nx][ny] = result[x][y] + 1
                queue.append((nx, ny))
    
    return result
```

### Complexity
- **Time Complexity:** O(m * n), more efficient as it maintains a single BFS pass for all cells.
- **Space Complexity:** O(m * n), for the result matrix.

---

## Approach 3: Dynamic Programming

### Intuition
This DP-based approach calculates distances by iterating in two passes, top-left to bottom-right and then bottom-right to top-left, accumulating distances either cell-wise downward or upward. This simultaneous update results in capturing the shortest path through distances comparison.

### Steps:
1. Initialize a result matrix with large values for "1".
2. Perform a forward pass (top-left to bottom-right), using previous results to compute current cell distances.
3. Conduct a backward pass (bottom-right to top-left) to further update minimal distances.

### Code
```python
def updateMatrix(matrix):
    rows, cols = len(matrix), len(matrix[0])
    result = [[float('inf')] * cols for _ in range(rows)]
    
    # Forward pass
    for r in range(rows):
        for c in range(cols):
            if matrix[r][c] == 0:
                result[r][c] = 0
            else:
                if r > 0:
                    result[r][c] = min(result[r][c], result[r-1][c] + 1)
                if c > 0:
                    result[r][c] = min(result[r][c], result[r][c-1] + 1)
    
    # Backward pass
    for r in range(rows-1, -1, -1):
        for c in range(cols-1, -1, -1):
            if r < rows - 1:
                result[r][c] = min(result[r][c], result[r+1][c] + 1)
            if c < cols - 1:
                result[r][c] = min(result[r][c], result[r][c+1] + 1)
    
    return result
```

### Complexity
- **Time Complexity:** O(m * n), through fixed iterations and cell updates in pair passes.
- **Space Complexity:** O(1), reusing input without incurring extra space upon input size.

These approaches balance between brute force simplicity, efficient parallel search using BFS, and direct updates via DP. With complexities outlined, the BFS from all "0"s approach balances comprehensiveness and optimality best in practical scenarios.

