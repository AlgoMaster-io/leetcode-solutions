# 63. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Approach 1: Dynamic Programming with a 2D Array

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m * n)
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        
        # If the start or end is an obstacle, no paths are possible
        if obstacleGrid[0][0] == 1 or obstacleGrid[m - 1][n - 1] == 1:
            return 0

        dp = [[0] * n for _ in range(m)]
        dp[0][0] = 1  # Start position

        # Initialize first column
        for i in range(1, m):
            dp[i][0] = 1 if obstacleGrid[i][0] == 0 and dp[i - 1][0] == 1 else 0

        # Initialize first row
        for j in range(1, n):
            dp[0][j] = 1 if obstacleGrid[0][j] == 0 and dp[0][j - 1] == 1 else 0

        # Fill the DP array
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 0:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
                else:
                    dp[i][j] = 0  # An obstacle

        return dp[m - 1][n - 1]  # The bottom-right corner
```

## Approach 2: Dynamic Programming with a 1D Array

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(n)
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        
        # Array to store current row's path counts
        dp = [0] * n

        dp[0] = 1 if obstacleGrid[0][0] == 0 else 0  # Start position

        # Fill for the first row
        for j in range(1, n):
            dp[j] = dp[j - 1] if obstacleGrid[0][j] == 0 else 0

        # Process the rest of the rows
        for i in range(1, m):
            # Update first column of the current row
            dp[0] = 1 if obstacleGrid[i][0] == 0 and dp[0] == 1 else 0
            for j in range(1, n):
                if obstacleGrid[i][j] == 0:
                    dp[j] += dp[j - 1]  # Current cell is a free path
                else:
                    dp[j] = 0  # Obstacle found, no paths through this cell

        return dp[n - 1]  # The bottom-right corner
```

