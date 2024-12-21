# [Leetcode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming Approach (Bottom-Up)](#dynamic-programming-approach-bottom-up)
3. [Dynamic Programming Approach (In-Place)](#dynamic-programming-approach-in-place)

## Recursive Approach

### Intuition:
The recursive approach is the most straightforward way to tackle this problem. Imagine that you need to move from the top-left to the bottom-right of the grid. At each cell `(i, j)`, you have the choice to move either right to `(i, j+1)` or down to `(i+1, j)`. The recursive solution explores both these paths and keeps track of the minimum sum encountered.

### Code:
```java
class Solution {
    public int minPathSum(int[][] grid) {
        return minPathSumHelper(grid, 0, 0);
    }
    
    private int minPathSumHelper(int[][] grid, int row, int col) {
        // Base condition: if we've reached the bottom-right corner, return its value
        if (row == grid.length - 1 && col == grid[0].length - 1) {
            return grid[row][col];
        }
        
        // If we've reached the last row, we can only move right
        if (row == grid.length - 1) {
            return grid[row][col] + minPathSumHelper(grid, row, col + 1);
        }
        
        // If we've reached the last column, we can only move down
        if (col == grid[0].length - 1) {
            return grid[row][col] + minPathSumHelper(grid, row + 1, col);
        }
        
        // Option to move right or down, take the path with the minimum sum
        int right = minPathSumHelper(grid, row, col + 1);
        int down = minPathSumHelper(grid, row + 1, col);
        
        return grid[row][col] + Math.min(right, down);
    }
}
```

### Complexity:
- **Time Complexity**: O(2^(m+n)) where m is the number of rows and n is the number of columns. This is because at each cell, you have two choices, so it's exponential.
- **Space Complexity**: O(m+n) for the recursion stack.

## Dynamic Programming Approach (Bottom-Up)

### Intuition:
This approach builds on the recursive solution but uses dynamic programming to avoid repeated calculations. The idea is to work backwards from the destination (bottom-right) cell and fill in a table with the minimum path sum to reach the bottom-right from any given cell.

### Code:
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        int[][] dp = new int[m][n];
        
        // Start from the destination cell
        dp[m - 1][n - 1] = grid[m - 1][n - 1];
        
        // Fill the last row (can only come from the right)
        for (int col = n - 2; col >= 0; col--) {
            dp[m - 1][col] = grid[m - 1][col] + dp[m - 1][col + 1];
        }
        
        // Fill the last column (can only come from below)
        for (int row = m - 2; row >= 0; row--) {
            dp[row][n - 1] = grid[row][n - 1] + dp[row + 1][n - 1];
        }
        
        // Fill the rest of the table
        for (int row = m - 2; row >= 0; row--) {
            for (int col = n - 2; col >= 0; col--) {
                dp[row][col] = grid[row][col] + Math.min(dp[row + 1][col], dp[row][col + 1]);
            }
        }
        
        return dp[0][0];
    }
}
```

### Complexity:
- **Time Complexity**: O(m*n) as we iterate over each cell exactly once.
- **Space Complexity**: O(m*n) for the `dp` table.

## Dynamic Programming Approach (In-Place)

### Intuition:
We observe that to compute the value at any cell `(i, j)`, we only need the values of the cell directly to the right `(i, j+1)` and the cell directly below `(i+1, j)` in the `dp` table. This allows us to actually use the input grid itself as our `dp` table to save space.

### Code:
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // Start from the destination cell and modify the grid in-place
        for (int row = m - 1; row >= 0; row--) {
            for (int col = n - 1; col >= 0; col--) {
                // If we're at the bottom-right corner, do nothing
                if (row == m - 1 && col == n - 1) continue;
                
                // If we're at the last row, we can only move right
                if (row == m - 1) {
                    grid[row][col] += grid[row][col + 1];
                }
                // If we're at the last column, we can only move down
                else if (col == n - 1) {
                    grid[row][col] += grid[row + 1][col];
                }
                // Otherwise, choose the minimum path from right and down
                else {
                    grid[row][col] += Math.min(grid[row + 1][col], grid[row][col + 1]);
                }
            }
        }
        
        return grid[0][0];
    }
}
```

### Complexity:
- **Time Complexity**: O(m*n) as we iterate over each cell exactly once.
- **Space Complexity**: O(1) since we use the input grid itself to store minimum path sums.

The in-place dynamic programming approach is the most optimal in terms of space usage, while the time complexity remains the same across both dynamic programming solutions.

