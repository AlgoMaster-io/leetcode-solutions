### [Leetcode Problem 63: Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

### Navigation
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)
- [Approach 3: Dynamic Programming with Tabulation](#approach-3-dynamic-programming-with-tabulation)
- [Approach 4: Dynamic Programming with Space Optimization](#approach-4-dynamic-programming-with-space-optimization)

### Approach 1: Recursive Approach

#### Intuition
The problem can be solved by exploring every possible path from the start to the destination. In a recursive approach, we recursively explore each possible step from the source to the destination point while considering obstacles. This naive approach has significant overhead due to the redundant calculations and is not efficient for larger grid sizes.

#### C++ Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        return countPaths(obstacleGrid, 0, 0);
    }
    
    int countPaths(const vector<vector<int>>& grid, int i, int j) {
        // If we are out of bounds or hit an obstacle, no path is possible
        if (i >= grid.size() || j >= grid[0].size() || grid[i][j] == 1) {
            return 0;
        }
        // If we reach the bottom-right, count as a valid path
        if (i == grid.size() - 1 && j == grid[0].size() - 1) {
            return 1;
        }
        // Explore both down and right paths
        return countPaths(grid, i + 1, j) + countPaths(grid, i, j + 1);
    }
};
```

#### Time and Space Complexity
- **Time Complexity:** \(O(2^{m+n})\): Each path explores two possibilities (right and down) at each step.
- **Space Complexity:** \(O(m+n)\): Recursive call stack could go as deep as \(m+n\).

### Approach 2: Dynamic Programming with Memoization

#### Intuition
In the recursive approach, many identical subproblems are recalculated multiple times. We can optimize by storing the results of subproblems in a memoization table so that we only compute each state once.

#### C++ Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> memo(m, vector<int>(n, -1));
        return helper(obstacleGrid, 0, 0, memo);
    }
    
    int helper(const vector<vector<int>>& grid, int i, int j, vector<vector<int>>& memo) {
        if (i >= grid.size() || j >= grid[0].size() || grid[i][j] == 1) {
            return 0;
        }
        if (i == grid.size() - 1 && j == grid[0].size() - 1) {
            return 1;
        }
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        // Save the computed value in memo table
        return memo[i][j] = helper(grid, i + 1, j, memo) + helper(grid, i, j + 1, memo);
    }
};
```

#### Time and Space Complexity
- **Time Complexity:** \(O(m \times n)\): Each cell computation is stored after being solved once.
- **Space Complexity:** \(O(m \times n)\): For the memoization table and recursive call stack.

### Approach 3: Dynamic Programming with Tabulation

#### Intuition
By building a dynamic programming table, we can solve the problem iteratively. The dp table represents the number of unique paths to each cell. If there's an obstacle, that cell will be 0, indicating no paths through it.

#### C++ Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        if (obstacleGrid[0][0] == 1) return 0; // If the start point is an obstacle

        // Create a dp table
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = 1;
        
        // Fill in the DP table
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    if (i > 0) dp[i][j] += dp[i-1][j];
                    if (j > 0) dp[i][j] += dp[i][j-1];
                }
            }
        }
        // The destination point will have the number of unique paths
        return dp[m-1][n-1];
    }
};
```

#### Time and Space Complexity
- **Time Complexity:** \(O(m \times n)\): Iterate over each grid cell.
- **Space Complexity:** \(O(m \times n)\): Additional space for the dp table.

### Approach 4: Dynamic Programming with Space Optimization

#### Intuition
Since the DP value of a cell relies only on the current and the immediate previous row, we can optimize space by maintaining only two rows instead of the entire table.

#### C++ Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<int> dp(n, 0);
        dp[0] = obstacleGrid[0][0] == 0 ? 1 : 0;
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else if (j > 0) {
                    dp[j] += dp[j - 1];
                }
            }
        }
        
        return dp[n - 1];
    }
};
```

#### Time and Space Complexity
- **Time Complexity:** \(O(m \times n)\): Iterate over each grid cell.
- **Space Complexity:** \(O(n)\): Reduced space for a single row, optimizing the space requirement.

