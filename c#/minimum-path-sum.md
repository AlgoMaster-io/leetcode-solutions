## [Leetcode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

### Approaches
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Dynamic Programming with Space Optimization](#dynamic-programming-with-space-optimization)

---

### Recursive Approach

**Intuition:**

The recursive approach involves considering all possible paths to reach the bottom-right corner, starting from the top-left corner. At every cell, you make a choice: either move right or down. You then compute the path sum for each choice recursively and take the minimum. 

The overhead is that the same results are computed repeatedly since the paths overlap due to branching - thus resulting in an exponential time complexity.

**Solution:**

```csharp
public class Solution {
    public int MinPathSum(int[][] grid) {
        return CalculateMinPath(grid, 0, 0);
    }

    private int CalculateMinPath(int[][] grid, int row, int col) {
        // If out of bounds, return an infinite sum to ensure it's not considered
        if (row >= grid.Length || col >= grid[0].Length) return int.MaxValue;

        // If reached bottom-right, return its value
        if (row == grid.Length - 1 && col == grid[0].Length - 1) return grid[row][col];

        // Otherwise, calculate the minimum path sum of right and down cells
        int right = CalculateMinPath(grid, row, col + 1);
        int down = CalculateMinPath(grid, row + 1, col);

        // Return the current cell value added to the minimum of the two possibilities
        return grid[row][col] + Math.Min(right, down);
    }
}
```

- **Time Complexity:** \(O(2^{m+n})\), due to exploring each possible path.
- **Space Complexity:** \(O(m + n)\), for the call stack during recursion.

---

### Dynamic Programming Approach

**Intuition:**

The recursive method has a lot of overlapping subproblems. We can exploit these overlaps using dynamic programming. By storing previously computed results in a dp array, we can avoid redundant computations, resulting in polynomial time complexity.

The dp array will store the minimum path sum required to reach each cell. Start by filling the top-left cell, then progressively fill the dp cells by considering the minimum cost of reaching from either the top or left cells.

**Solution:**

```csharp
public class Solution {
    public int MinPathSum(int[][] grid) {
        int m = grid.Length, n = grid[0].Length;
        var dp = new int[m, n];
        
        // Fill the DP table
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    dp[i, j] = grid[i][j];
                } else {
                    int fromTop = i > 0 ? dp[i - 1, j] : int.MaxValue;
                    int fromLeft = j > 0 ? dp[i, j - 1] : int.MaxValue;
                    dp[i, j] = grid[i][j] + Math.Min(fromTop, fromLeft);
                }
            }
        }
        return dp[m - 1, n - 1];
    }
}
```

- **Time Complexity:** \(O(m \times n)\), we process each cell once.
- **Space Complexity:** \(O(m \times n)\), for the dp array.

---

### Dynamic Programming with Space Optimization

**Intuition:**

Instead of maintaining a full 2D dp array, we recognize that at any point we only need the current and previous row to compute the current cell minimum path sum. Thus, we can optimize space by using a single 1D array.

**Solution:**

```csharp
public class Solution {
    public int MinPathSum(int[][] grid) {
        int m = grid.Length, n = grid[0].Length;
        var dp = new int[n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    dp[j] = grid[i][j];
                } else if (i == 0) {
                    dp[j] = dp[j - 1] + grid[i][j];
                } else if (j == 0) {
                    dp[j] = dp[j] + grid[i][j];
                } else {
                    dp[j] = Math.Min(dp[j - 1], dp[j]) + grid[i][j];
                }
            }
        }
        return dp[n - 1];
    }
}
```

- **Time Complexity:** \(O(m \times n)\), we process each cell once.
- **Space Complexity:** \(O(n)\), using a single array for the current row's minimum path sums.

