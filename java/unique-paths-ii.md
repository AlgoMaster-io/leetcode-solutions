# [Leetcode 63: Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Table of Contents
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Memoization](#approach-2-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)
- [Approach 4: Dynamic Programming with Space Optimization](#approach-4-dynamic-programming-with-space-optimization)

---

## Approach 1: Recursive Backtracking

**Intuition**:  
The recursive backtracking approach is a brute force method where we explore every possible path from the starting point to the destination, taking into account obstacles. This involves trying to move right and down at each step recursively.

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        return countPaths(0, 0, obstacleGrid);
    }
    
    // Helper method to recursively explore paths
    private int countPaths(int row, int col, int[][] grid) {
        // Base cases
        if (row >= grid.length || col >= grid[0].length || grid[row][col] == 1) {
            return 0; // Out of bounds or hit an obstacle
        }
        if (row == grid.length - 1 && col == grid[0].length - 1) {
            return 1; // Reached destination
        }
        // Explore both directions: right and down
        int rightPaths = countPaths(row, col + 1, grid);
        int downPaths = countPaths(row + 1, col, grid);
        return rightPaths + downPaths; // Total paths from current position
    }
}
```

- **Time Complexity**: O(2^(m+n)) — Each point can either move right or down, resulting in exponential time.
- **Space Complexity**: O(m+n) — Recursive stack space.

---

## Approach 2: Memoization

**Intuition**:  
Memoization will be used to store results of already computed paths for each grid cell to avoid redundant calculations. This is an optimization to the recursive approach.

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int[][] memo = new int[obstacleGrid.length][obstacleGrid[0].length];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return countPaths(0, 0, obstacleGrid, memo);
    }
    
    private int countPaths(int row, int col, int[][] grid, int[][] memo) {
        if (row >= grid.length || col >= grid[0].length || grid[row][col] == 1) {
            return 0;
        }
        if (row == grid.length - 1 && col == grid[0].length - 1) {
            return 1;
        }
        if (memo[row][col] != -1) {
            return memo[row][col];
        }
        int rightPaths = countPaths(row, col + 1, grid, memo);
        int downPaths = countPaths(row + 1, col, grid, memo);
        memo[row][col] = rightPaths + downPaths;
        return memo[row][col];
    }
}
```

- **Time Complexity**: O(m*n) — Each cell is computed once.
- **Space Complexity**: O(m*n) + O(m+n) — Space for memo array and recursion stack.

---

## Approach 3: Dynamic Programming

**Intuition**:  
Using a table to systematically compute the number of unique paths to each cell, considering obstacles.

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] dp = new int[m][n];

        // Initialize the first cell
        dp[0][0] = obstacleGrid[0][0] == 0 ? 1 : 0;

        // Fill the first column
        for (int i = 1; i < m; i++) {
            dp[i][0] = obstacleGrid[i][0] == 0 ? dp[i-1][0] : 0;
        }
        
        // Fill the first row
        for (int j = 1; j < n; j++) {
            dp[0][j] = obstacleGrid[0][j] == 0 ? dp[0][j-1] : 0;
        }

        // Fill the rest of the dp table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }

        return dp[m-1][n-1];
    }
}
```

- **Time Complexity**: O(m*n) — We iterate through each cell once.
- **Space Complexity**: O(m*n) — Space for the dp array.

---

## Approach 4: Dynamic Programming with Space Optimization

**Intuition**:  
By leveraging the fact that we only need the previous row and previous column to calculate the current cell's paths, we can optimize the space to O(n) using a single array.

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[] dp = new int[n];
        
        // Initialize the first cell
        dp[0] = obstacleGrid[0][0] == 0 ? 1 : 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else if (j > 0) {
                    dp[j] += dp[j-1];
                }
            }
        }
        
        return dp[n-1];
    }
}
```

- **Time Complexity**: O(m*n) — We iterate through each cell once.
- **Space Complexity**: O(n) — Space for the 1-D dp array.

