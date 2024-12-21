# [LeetCode 741: Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

## Approaches

- [Approach 1: Two-Pass DFS (Recursive with Memoization)](#approach-1-two-pass-dfs)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming)

---

### Approach 1: Two-Pass DFS (Recursive with Memoization)

#### Intuition

In this approach, the idea is to simulate two traversals across the grid:
1. Traverse from the start (0, 0) to the end (n-1, n-1) and collect cherries.
2. Traverse back from the end (n-1, n-1) to the start (0, 0) and collect cherries again (cannot collect already picked).

This method works recursively using a DFS combined with memoization to cache results of overlapping subproblems, reducing repeated calculations.

#### Detailed Steps:

1. Define a recursive function `dfs(r1, c1, r2, c2)` where `(r1, c1)` and `(r2, c2)` represent the positions of two people traversing the grid. These positions simulate both passes.
2. Check for bounds and obstacles. If any position is out of bounds or hits a thorn (`grid[r][c] == -1`), return a very large negative number to signify an impossible path.
3. If both have reached the bottom-right corner, check whether one or both have picked cherries, and return their sum.
4. If the result for `dfs(r1, c1, r2, c2)` is already computed, use it from the memoization table.
5. Calculate the current number of cherries: If both are on the same cell, count it once, otherwise count for both cells.
6. Move both in four possible directions: right-right, right-down, down-right, down-down.
7. Cache the maximum cherries collected in `memo` and return the value.

#### Code

```csharp
public class Solution {
    private int[,] grid;
    private int n;
    private int[,,,] memo;
    private int[][] directions = new int[][] {
        new int[] {0, 1}, new int[] {1, 0}
    };

    public int CherryPickup(int[][] grid) {
        this.grid = grid;
        this.n = grid.Length;
        memo = new int[n, n, n, n];

        int result = Math.Max(0, dfs(0, 0, 0));
        return result;
    }

    private int dfs(int r1, int c1, int r2) {
        int c2 = r1 + c1 - r2;
        if (r1 >= n || c1 >= n || r2 >= n || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1) {
            return int.MinValue;
        }
        if (r1 == n-1 && c1 == n-1) {
            return grid[r1][c1];
        }
        if (memo[r1, c1, r2, c2] != 0) {
            return memo[r1, c1, r2, c2];
        }
        int cherries = grid[r1][c1];
        if (r1 != r2 || c1 != c2) {
            cherries += grid[r2][c2];
        }

        int nextMax = Math.Max(
            Math.Max(dfs(r1 + 1, c1, r2 + 1), dfs(r1, c1 + 1, r2 + 1)),
            Math.Max(dfs(r1 + 1, c1, r2), dfs(r1, c1 + 1, r2))
        );

        cherries += nextMax;
        memo[r1, c1, r2, c2] = cherries;
        return cherries;
    }
}
```

#### Complexity Analysis

- **Time Complexity:** \(O(n^3)\), due to three loops over indices up to n.
- **Space Complexity:** \(O(n^3)\), storing results in a memoization table.

---

### Approach 2: Dynamic Programming (Bottom-Up)

#### Intuition

This approach leverages dynamic programming using a bottom-up strategy. It simplifies the state from the recursive approach by using a 3-dimensional DP table where `dp[r1][c1][c2]` represents the maximum cherries collected when both "robots" are at `(r1, c1)` and `(r2, c2)` coming from `(0, 0)`.

#### Detailed Steps:

1. Initialize a 3D DP array with dimensions `[n][n][n]` setting all cells to negative infinity initially, representing unfulfilled states.
2. Set `dp[0][0][0]` to `grid[0][0]` as the starting state.
3. Iterate through all row and column indices possible within bounds.
4. For each state `(r1, c1, c2)`, compute `r2` as `r1 + c1 - c2`.
5. Skip any invalid positions or thorn cells.
6. Update `dp[r1][c1][c2]` by taking into consideration 4 possible previous states and adding the current cherries.
7. Account for collecting cherries if both robots are on the same cell.
8. The result is the value `dp[n-1][n-1][n-1]` (unless it's negative infinity indicating impossibility).

#### Code

```csharp
public class Solution {
    public int CherryPickup(int[][] grid) {
        int n = grid.Length;
        int[,,] dp = new int[n, n, n];
        
        for (int r1 = 0; r1 < n; ++r1) {
            for (int c1 = 0; c1 < n; ++c1) {
                for (int r2 = 0; r2 < n; ++r2) {
                    dp[r1, c1, r2] = int.MinValue;
                }
            }
        }
        
        dp[0, 0, 0] = grid[0][0];
        
        for (int r1 = 0; r1 < n; ++r1) {
            for (int c1 = 0; c1 < n; ++c1) {
                for (int r2 = 0; r2 < n; ++r2) {
                    int c2 = r1 + c1 - r2;
                    if (c2 < 0 || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1) {
                        continue;
                    }
                    
                    int currentCherries = grid[r1][c1];
                    if (r1 != r2 || c1 != c2) {
                        currentCherries += grid[r2][c2];
                    }
                    
                    if (r1 > 0) {
                        dp[r1, c1, r2] = Math.Max(dp[r1, c1, r2], dp[r1 - 1, c1, r2]);
                    }
                    if (c1 > 0) {
                        dp[r1, c1, r2] = Math.Max(dp[r1, c1, r2], dp[r1, c1 - 1, r2]);
                    }
                    if (r2 > 0) {
                        dp[r1, c1, r2] = Math.Max(dp[r1, c1, r2], dp[r1, c1, r2 - 1]);
                    }
                    if (c2 > 0) {
                        dp[r1, c1, r2] = Math.Max(dp[r1, c1, r2], dp[r1, c1 - 1, r2]);
                    }
                    
                    dp[r1, c1, r2] += currentCherries;
                }
            }
        }
        
        return Math.Max(0, dp[n - 1, n - 1, n - 1]);
    }
}
```

#### Complexity Analysis

- **Time Complexity:** \(O(n^3)\), iterating through all grid positions.
- **Space Complexity:** \(O(n^3)\), storing values in a DP table.

Both methods strategically handle complex recursion and overlapping subproblems either through memoization or a tabular form of dynamic programming. The primary challenge is managing state transitions and collecting the maximum number of cherries efficiently, even when faced with thorns.

