# [Unique Paths II - Leetcode Problem 63](https://leetcode.com/problems/unique-paths-ii/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Optimized Dynamic Programming with Space Optimization](#optimized-dynamic-programming-approach)

### Recursive Approach

#### Intuition:
The basic idea is to use recursion to explore all possible paths from the starting point (0, 0) to the target point (m-1, n-1) while accounting for obstacles. If a cell has an obstacle, no path can pass through it.

#### Code:
```python
def uniquePathsWithObstacles(obstacleGrid):
    def pathCount(x, y):
        # Return zero if out of bounds or on an obstacle
        if x >= len(obstacleGrid) or y >= len(obstacleGrid[0]) or obstacleGrid[x][y] == 1:
            return 0
        # If the bottom-right corner is reached
        if x == len(obstacleGrid) - 1 and y == len(obstacleGrid[0]) - 1:
            return 1
        # Count paths from the cell to the right and the cell below
        return pathCount(x + 1, y) + pathCount(x, y + 1)

    return pathCount(0, 0)

# Time Complexity: O(2^(m+n))
# Space Complexity: O(m+n) due to the recursion stack
```

### Dynamic Programming Approach

#### Intuition:
We avoid the exponential complexity of recursion by using a 2D array `dp` where `dp[i][j]` represents the number of unique paths to reach cell (i, j) from (0, 0). We fill the array iteratively.

1. `dp[0][0]` should be 1 if there is no obstacle at the start.
2. Fill `dp[i][0]` (first column) and `dp[0][j]` (first row) by checking if the last cell is reachable.
3. For other cells, if there is no obstacle, the value is the sum of the ways to reach from the top (`dp[i-1][j]`) and from the left (`dp[i][j-1]`).

#### Code:
```python
def uniquePathsWithObstacles(obstacleGrid):
    if not obstacleGrid or obstacleGrid[0][0] == 1:
        return 0
    
    m, n = len(obstacleGrid), len(obstacleGrid[0])
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = 1

    # Fill the first column
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] if obstacleGrid[i][0] == 0 else 0
    
    # Fill the first row
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] if obstacleGrid[0][j] == 0 else 0

    # Fill the rest of the dp table
    for i in range(1, m):
        for j in range(1, n):
            if obstacleGrid[i][j] == 0:
                dp[i][j] = dp[i-1][j] + dp[i][j-1]

    return dp[m-1][n-1]

# Time Complexity: O(m*n) where m is the number of rows and n is the number of columns
# Space Complexity: O(m*n) for the dp table
```

### Optimized Dynamic Programming Approach

#### Intuition:
We can further optimize the space complexity by using a single array. As we only depend on the current and previous row's values, we only need to keep track of one column at a time.

#### Code:
```python
def uniquePathsWithObstacles(obstacleGrid):
    if not obstacleGrid or obstacleGrid[0][0] == 1:
        return 0
    
    m, n = len(obstacleGrid), len(obstacleGrid[0])
    dp = [0] * n
    dp[0] = 1

    for i in range(m):
        for j in range(n):
            # If there's an obstacle, no path should be counted
            if obstacleGrid[i][j] == 1:
                dp[j] = 0
            # If there's no obstacle, calculate the number of paths
            elif j > 0:
                dp[j] += dp[j-1]  # Add the number of ways to reach from the left cell

    return dp[-1]

# Time Complexity: O(m*n)
# Space Complexity: O(n) for the dp array representing the current row
```

