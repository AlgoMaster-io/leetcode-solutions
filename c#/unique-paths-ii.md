# [Leetcode 63: Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Approaches
- [Approach 1: Recursive Approach with memoization](#approach-1)
- [Approach 2: Dynamic Programming](#approach-2)

---

## Approach 1: Recursive Approach with Memoization

The problem statement is to find a path from the top-left corner to the bottom-right corner of a 2D grid, given that some cells contain obstacles and are impassable. We'll begin with a recursive approach and use memoization to optimize it.

### Intuition

1. **Base Case**: 
   - If the robot is outside the grid, return 0.
   - If the robot hits an obstacle, return 0.
   - If the robot is at the starting position and it's not an obstacle, there is exactly 1 way to be there.
   
2. **Recursive Relation**:
   - The number of ways to get to a cell `(i, j)` can be determined by the number of ways to get to `(i-1, j)` (from the top) and `(i, j-1)` (from the left).

3. **Memoization**:
   - Store the results of the computed paths for each cell to avoid redundant calculations.

### Implementation

```csharp
public class Solution {
    public int UniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.Length;
        int n = obstacleGrid[0].Length;
        int[,] memo = new int[m, n];
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                memo[i, j] = -1; // Initialize memoization array
            }
        }
        
        return UniquePathsHelper(obstacleGrid, m - 1, n - 1, memo);
    }
    
    private int UniquePathsHelper(int[][] grid, int i, int j, int[,] memo) {
        if (i < 0 || j < 0 || grid[i][j] == 1) {
            return 0;
        }
        
        if (i == 0 && j == 0) {
            return 1;
        }
        
        if (memo[i, j] != -1) {
            return memo[i, j];
        }
        
        int pathsFromTop = UniquePathsHelper(grid, i - 1, j, memo);
        int pathsFromLeft = UniquePathsHelper(grid, i, j - 1, memo);
        
        memo[i, j] = pathsFromTop + pathsFromLeft;
        return memo[i, j];
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(m * n)` because we calculate each cell's paths only once.
- **Space Complexity**: `O(m * n)` due to the recursion stack and memoization table.

---

## Approach 2: Dynamic Programming

### Intuition

Dynamic programming is an optimized approach of the recursive method that uses tabulation instead of recursion to store the number of ways to reach each cell in a DP table.

1. **Initialization**:
   - Start filling the dp array where `dp[i][0]` and `dp[0][j]` depend on whether there is any obstacle before them in their row/column.

2. **Transition**:
   - For other cells, `dp[i][j]` holds the sum of `dp[i-1][j]` and `dp[i][j-1]` if there's no obstacle at `(i, j)`.

### Implementation

```csharp
public class Solution {
    public int UniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.Length;
        int n = obstacleGrid[0].Length;
        
        if (obstacleGrid[0][0] == 1) return 0;
        
        int[,] dp = new int[m, n];
        dp[0, 0] = 1; // Starting point, always 1 if there's no obstacle.
        
        // Initialize the first column
        for (int i = 1; i < m; i++) {
            dp[i, 0] = (obstacleGrid[i][0] == 0) && (dp[i-1, 0] == 1) ? 1 : 0;
        }
        
        // Initialize the first row
        for (int j = 1; j < n; j++) {
            dp[0, j] = (obstacleGrid[0][j] == 0) && (dp[0, j-1] == 1) ? 1 : 0;
        }
        
        // Fill the rest of the dp table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i, j] = dp[i - 1, j] + dp[i, j - 1];
                }
                // Else it remains 0 as default initialized
            }
        }
        
        return dp[m - 1, n - 1];
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(m * n)` as we fill each cell in the DP table once.
- **Space Complexity**: `O(m * n)` since we store the results for each cell in a separate array.

By following these approaches, you can transform the problem into a manageable form whether opting for a recursive or dynamic programming technique. The transition from naive solutions to more optimized ones ensures that you solve larger inputs efficiently.

