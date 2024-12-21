# [Leetcode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approaches:
- [Brute Force Approach: Recursive Solution](#brute-force-approach-recursive-solution)
- [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)
- [Optimized Dynamic Programming with Tabulation](#optimized-dynamic-programming-with-tabulation)

---

### Brute Force Approach: Recursive Solution

To solve the problem recursively, we try every possible path from the top-left to the bottom-right corner. This approach explores all the paths, thus has a high time complexity due to repeated calculations.

#### Intuition:
1. Starting at the top-left corner of the grid, we try to move either down or right until we reach the bottom-right corner.
2. For each cell, the cost to reach that cell is the minimum of the path sums of the cell directly above it or directly to the left of it, plus the cost of the current cell.
3. As a base condition, when the target cell (bottom-right) is reached, we return the cost at that cell.

#### Code:

```python
def minPathSum(grid):
    def calculate_min_sum(i, j):
        # Base case: If we are at the top-left corner, we just return its value.
        if i == 0 and j == 0:
            return grid[0][0]
        
        # If we go out of boundaries, return infinity
        if i < 0 or j < 0:
            return float('inf')
        
        # Calculate minimum path sum via top and left
        return grid[i][j] + min(calculate_min_sum(i-1, j), calculate_min_sum(i, j-1))
    
    # Start the recursion from bottom-right corner
    return calculate_min_sum(len(grid) - 1, len(grid[0]) - 1)
```

**Time Complexity:** O(2^(m+n)), where m is the number of rows and n is the number of columns (exponential because we are recalculating paths).

**Space Complexity:** O(m + n), due to the recursion stack.

---

### Dynamic Programming with Memoization

We optimize the recursive solution using memoization to store previously calculated minimum path sums for each cell, which avoids redundant calculations.

#### Intuition:
1. Instead of recalculating the minimum path for each cell multiple times, store the result in a memo table for reuse.
2. Reuse previously computed results to build upon for more significant problem subparts.

#### Code:

```python
def minPathSum(grid):
    memo = {}
    
    def calculate_min_sum(i, j):
        # Base case: If we are at the top-left corner, we just return its value.
        if i == 0 and j == 0:
            return grid[0][0]
        
        # If we go out of boundaries, return infinity
        if i < 0 or j < 0:
            return float('inf')
        
        # Check if already computed
        if (i, j) in memo:
            return memo[(i, j)]
        
        # Compute the minimum path sum via top and left
        memo[(i, j)] = grid[i][j] + min(calculate_min_sum(i-1, j), calculate_min_sum(i, j-1))
        
        return memo[(i, j)]
    
    # Start the recursion from bottom-right corner
    return calculate_min_sum(len(grid) - 1, len(grid[0]) - 1)
```

**Time Complexity:** O(m * n), where m is the number of rows and n is the number of columns.

**Space Complexity:** O(m * n), due to memoization storage.

---

### Optimized Dynamic Programming with Tabulation

The most advanced and efficient approach uses dynamic programming with tabulation to iteratively calculate the minimum path sum bottom-up and fill a 2D dp array.

#### Intuition:
1. Use a 2D dp array where dp[i][j] represents the minimum path sum to reach cell (i, j).
2. Base case: Initialize dp[0][0] with grid[0][0].
3. Fill the first row and first column since they have only one possible way of reaching them (from the left or top, respectively).
4. For the rest of the grid, populate dp[i][j] as `grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.
5. The final answer will be in dp[m-1][n-1].

#### Code:

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0] * n for _ in range(m)]
    
    # Initialize the starting point
    dp[0][0] = grid[0][0]
    
    # Fill the first row
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
    
    # Fill the first column
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
    
    # Fill the rest of the dp array
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
    
    return dp[m-1][n-1]
```

**Time Complexity:** O(m * n).

**Space Complexity:** O(m * n), due to dp table.

This solution can still be optimized to use O(n) space by maintaining only two rows: the current and previous rows.

---

