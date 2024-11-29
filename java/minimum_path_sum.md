# 64. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approach 1: Recursion (Brute force)

### Solution
```java
// Time Complexity: O(2^(m+n))
// Space Complexity: O(m+n) - due to the recursion stack
public class Solution {
    public int minPathSum(int[][] grid) {
        return calculate(grid, grid.length - 1, grid[0].length - 1);
    }
    
    // Recursive function to find the minimum path sum to reach cell (i, j)
    private int calculate(int[][] grid, int i, int j) {
        // Base cases for boundaries
        if (i == 0 && j == 0) return grid[0][0]; // Start point
        if (i < 0 || j < 0) return Integer.MAX_VALUE; // Out of grid

        // Calculate minimum path sum to current cell from either top or left
        return grid[i][j] + Math.min(calculate(grid, i - 1, j), calculate(grid, i, j - 1));
    }
}
```

## Approach 2: Recursion with Memoization

### Solution
```java
// Time Complexity: O(m*n)
// Space Complexity: O(m*n) - for the memoization table
public class Solution {
    public int minPathSum(int[][] grid) {
        int[][] memo = new int[grid.length][grid[0].length];
        return calculate(grid, grid.length - 1, grid[0].length - 1, memo);
    }
    
    // Recursive function with memoization to find the minimum path sum to reach cell (i, j)
    private int calculate(int[][] grid, int i, int j, int[][] memo) {
        // Base cases for boundaries
        if (i == 0 && j == 0) return grid[0][0]; // Start point
        if (i < 0 || j < 0) return Integer.MAX_VALUE; // Out of grid

        if (memo[i][j] != 0) return memo[i][j]; // Return already computed result

        // Calculate minimum path sum to current cell from either top or left
        memo[i][j] = grid[i][j] + Math.min(calculate(grid, i - 1, j, memo), calculate(grid, i, j - 1, memo));
        return memo[i][j];
    }
}
```

## Approach 3: Dynamic Programming

### Solution
```java
// Time Complexity: O(m*n)
// Space Complexity: O(m*n)
public class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // Create a DP table to store the minimum path sum up to each cell
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0]; // Initialize the starting point
        
        // Fill the first row (can only come from the left)
        for (int i = 1; i < n; i++) {
            dp[0][i] = dp[0][i - 1] + grid[0][i];
        }

        // Fill the first column (can only come from above)
        for (int i = 1; i < m; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }

        // Fill the rest of the dp table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                // Minimum of coming from the top or the left
                dp[i][j] = grid[i][j] + Math.min(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[m - 1][n - 1]; // Return the last cell which contains the result
    }
}
```

## Approach 4: Dynamic Programming with Space Optimization

### Solution
```java
// Time Complexity: O(m*n)
// Space Complexity: O(n)
public class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // Use a 1D dp array to save space
        int[] dp = new int[n];
        dp[0] = grid[0][0]; // Initialize the starting point
        
        // Fill the first row (can only come from the left)
        for (int i = 1; i < n; i++) {
            dp[i] = dp[i - 1] + grid[0][i];
        }

        // Fill for each row using the dp array
        for (int i = 1; i < m; i++) {
            dp[0] += grid[i][0]; // Update the first column
            for (int j = 1; j < n; j++) {
                dp[j] = grid[i][j] + Math.min(dp[j], dp[j - 1]); // Update by choosing minimum of top or left
            }
        }

        return dp[n - 1]; // Return the last cell which contains the result
    }
}
```

