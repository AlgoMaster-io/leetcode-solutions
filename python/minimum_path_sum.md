# 64. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approach 1: Recursion (Brute force)

### Solution
python
```python
# Time Complexity: O(2^(m*n))
# Space Complexity: O(m+n) - due to the recursion stack

class Solution:
    def minPathSum(self, grid):
        return self.calculate(grid, len(grid) - 1, len(grid[0]) - 1)

    # Recursive function to find the minimum path sum to reach cell (i, j)
    def calculate(self, grid, i, j):
        # Base cases for boundaries
        if i == 0 and j == 0:
            return grid[0][0]  # Start point
        if i < 0 or j < 0:
            return float('inf')  # Out of grid
        
        # Calculate minimum path sum to current cell from either top or left
        return grid[i][j] + min(self.calculate(grid, i - 1, j), self.calculate(grid, i, j - 1))
```

## Approach 2: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(m*n)
# Space Complexity: O(m*n) - for the memoization table

class Solution:
    def minPathSum(self, grid):
        memo = [[0] * len(grid[0]) for _ in range(len(grid))]
        return self.calculate(grid, len(grid) - 1, len(grid[0]) - 1, memo)
    
    # Recursive function with memoization to find the minimum path sum to reach cell (i, j)
    def calculate(self, grid, i, j, memo):
        # Base cases for boundaries
        if i == 0 and j == 0:
            return grid[0][0]  # Start point
        if i < 0 or j < 0:
            return float('inf')  # Out of grid
        
        if memo[i][j] != 0:
            return memo[i][j]  # Return already computed result
        
        # Calculate minimum path sum to current cell from either top or left
        memo[i][j] = grid[i][j] + min(self.calculate(grid, i - 1, j, memo), self.calculate(grid, i, j - 1, memo))
        return memo[i][j]
```

## Approach 3: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(m*n)
# Space Complexity: O(m*n)

class Solution:
    def minPathSum(self, grid):
        m = len(grid)
        n = len(grid[0])
        
        # Create a DP table to store the minimum path sum up to each cell
        dp = [[0] * n for _ in range(m)]
        dp[0][0] = grid[0][0]  # Initialize the starting point
        
        # Fill the first row (can only come from the left)
        for i in range(1, n):
            dp[0][i] = dp[0][i - 1] + grid[0][i]
        
        # Fill the first column (can only come from above)
        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        
        # Fill the rest of the dp table
        for i in range(1, m):
            for j in range(1, n):
                # Minimum of coming from the top or the left
                dp[i][j] = grid[i][j] + min(dp[i - 1][j], dp[i][j - 1])
        
        return dp[m - 1][n - 1]  # Return the last cell which contains the result
```

## Approach 4: Dynamic Programming with Space Optimization

### Solution
python
```python
# Time Complexity: O(m*n)
# Space Complexity: O(n)

class Solution:
    def minPathSum(self, grid):
        m = len(grid)
        n = len(grid[0])
        
        # Use a 1D dp array to save space
        dp = [0] * n
        dp[0] = grid[0][0]  # Initialize the starting point
        
        # Fill the first row (can only come from the left)
        for i in range(1, n):
            dp[i] = dp[i - 1] + grid[0][i]

        # Fill for each row using the dp array
        for i in range(1, m):
            dp[0] += grid[i][0]  # Update the first column
            for j in range(1, n):
                dp[j] = grid[i][j] + min(dp[j], dp[j - 1])  # Update by choosing minimum of top or left
        
        return dp[n - 1]  # Return the last cell which contains the result
```

