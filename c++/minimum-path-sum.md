# [Leetcode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming (Memoization)](#approach-2-dynamic-programming-memoization)
- [Approach 3: Dynamic Programming (Tabulation - Bottom-Up Approach)](#approach-3-dynamic-programming-tabulation-bottom-up-approach)
- [Approach 4: Dynamic Programming (Optimized In-Place)](#approach-4-dynamic-programming-optimized-in-place)

---

### Approach 1: Recursive Solution

#### Intuition
At each cell (i, j), we can only move right or down. At the bottom-right corner of the grid, we have the end of our path. We define a recursive relation where the minimum path sum to reach cell (i, j) is the value of that cell plus the minimum of the path sums coming from the cell above it or from the left.

#### Code
```cpp
class Solution {
public:
    int minPathSumHelper(vector<vector<int>>& grid, int i, int j) {
        // Base case: when we're at the top-left corner cell
        if (i == 0 && j == 0) return grid[i][j];
        
        // If out of bounds, return an infinite-like value
        if (i < 0 || j < 0) return INT_MAX;
        
        // Minimum path sum: cell value + minimum of the path sums from top or left
        return grid[i][j] + min(minPathSumHelper(grid, i - 1, j), minPathSumHelper(grid, i, j - 1));
    }

    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        return minPathSumHelper(grid, m - 1, n - 1);
    }
};
```

#### Complexity
- **Time Complexity**: O(2^(m+n)), because each cell makes two recursive calls.
- **Space Complexity**: O(m + n), due to the recursion stack.

---

### Approach 2: Dynamic Programming (Memoization)

#### Intuition
To optimize the recursive solution, we use memoization. We store the results of the subproblems in a 2D array so that each subproblem is solved only once.

#### Code
```cpp
class Solution {
public:
    vector<vector<int>> memo;

    int minPathSumHelper(vector<vector<int>>& grid, int i, int j) {
        if (i == 0 && j == 0) return grid[i][j];
        if (i < 0 || j < 0) return INT_MAX;
        
        if (memo[i][j] != -1) return memo[i][j];
        
        memo[i][j] = grid[i][j] + min(minPathSumHelper(grid, i - 1, j), minPathSumHelper(grid, i, j - 1));
        return memo[i][j];
    }

    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Initialize memo array with -1 (meaning unsolved subproblems)
        memo = vector<vector<int>>(m, vector<int>(n, -1));
        
        return minPathSumHelper(grid, m - 1, n - 1);
    }
};
```

#### Complexity
- **Time Complexity**: O(m * n), each cell is solved at most once.
- **Space Complexity**: O(m * n), for the memoization table.

---

### Approach 3: Dynamic Programming (Tabulation - Bottom-Up Approach)

#### Intuition
Instead of solving the problem recursively, we'll solve it iteratively starting from the top-left corner. We fill out a dp table where each entry at (i, j) represents the minimum path sum to reach that cell.

#### Code
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        vector<vector<int>> dp(m, vector<int>(n, 0));
        
        // Set the starting point
        dp[0][0] = grid[0][0];
        
        // Fill the first row and the first column
        for(int i = 1; i < m; i++) dp[i][0] = dp[i-1][0] + grid[i][0];
        for(int j = 1; j < n; j++) dp[0][j] = dp[0][j-1] + grid[0][j];
        
        // Fill the rest of the dp table
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]);
            }
        }
        
        // The answer is the minimum path sum to the bottom-right corner
        return dp[m-1][n-1];
    }
};
```

#### Complexity
- **Time Complexity**: O(m * n), we iterate over all cells of the grid.
- **Space Complexity**: O(m * n), for the dp table.

---

### Approach 4: Dynamic Programming (Optimized In-Place)

#### Intuition
We can optimize space further by reusing the grid itself to store our calculations, thus eliminating the need for an additional dp table.

#### Code
```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Update the first column
        for(int i = 1; i < m; i++){
            grid[i][0] += grid[i-1][0];
        }
        // Update the first row
        for(int j = 1; j < n; j++){
            grid[0][j] += grid[0][j-1];
        }
        
        // Update the rest of the grid with the minimum paths
        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                grid[i][j] += min(grid[i-1][j], grid[i][j-1]);
            }
        }
        
        // The result is now stored in the bottom-right corner
        return grid[m-1][n-1];
    }
};
```

#### Complexity
- **Time Complexity**: O(m * n), same as previous DP solution.
- **Space Complexity**: O(1), no extra space is used, and calculations are done in-place.

