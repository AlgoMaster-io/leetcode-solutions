# 64. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approach 1: Recursion (Brute force)

### Solution
```cpp
// Time Complexity: O(2^(m+n))
// Space Complexity: O(m+n) - due to the recursion stack
#include <vector>
#include <algorithm>

class Solution {
public:
    int minPathSum(std::vector<std::vector<int>>& grid) {
        return calculate(grid, grid.size() - 1, grid[0].size() - 1);
    }
    
private:
    // Recursive function to find the minimum path sum to reach cell (i, j)
    int calculate(std::vector<std::vector<int>>& grid, int i, int j) {
        // Base cases for boundaries
        if (i == 0 && j == 0) return grid[0][0]; // Start point
        if (i < 0 || j < 0) return std::numeric_limits<int>::max(); // Out of grid

        // Calculate minimum path sum to current cell from either top or left
        return grid[i][j] + std::min(calculate(grid, i - 1, j), calculate(grid, i, j - 1));
    }
};
```

## Approach 2: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(m*n)
// Space Complexity: O(m*n) - for the memoization table
#include <vector>
#include <algorithm>

class Solution {
public:
    int minPathSum(std::vector<std::vector<int>>& grid) {
        std::vector<std::vector<int>> memo(grid.size(), std::vector<int>(grid[0].size(), 0));
        return calculate(grid, grid.size() - 1, grid[0].size() - 1, memo);
    }
    
private:
    // Recursive function with memoization to find the minimum path sum to reach cell (i, j)
    int calculate(std::vector<std::vector<int>>& grid, int i, int j, std::vector<std::vector<int>>& memo) {
        // Base cases for boundaries
        if (i == 0 && j == 0) return grid[0][0]; // Start point
        if (i < 0 || j < 0) return std::numeric_limits<int>::max(); // Out of grid

        if (memo[i][j] != 0) return memo[i][j]; // Return already computed result

        // Calculate minimum path sum to current cell from either top or left
        memo[i][j] = grid[i][j] + std::min(calculate(grid, i - 1, j, memo), calculate(grid, i, j - 1, memo));
        return memo[i][j];
    }
};
```

## Approach 3: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(m*n)
// Space Complexity: O(m*n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int minPathSum(std::vector<std::vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Create a DP table to store the minimum path sum up to each cell
        std::vector<std::vector<int>> dp(m, std::vector<int>(n));
        dp[0][0] = grid[0][0]; // Initialize the starting point
        
        // Fill the first row (can only come from the left)
        for (int i = 1; i < n; ++i) {
            dp[0][i] = dp[0][i - 1] + grid[0][i];
        }

        // Fill the first column (can only come from above)
        for (int i = 1; i < m; ++i) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }

        // Fill the rest of the dp table
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                // Minimum of coming from the top or the left
                dp[i][j] = grid[i][j] + std::min(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[m - 1][n - 1]; // Return the last cell which contains the result
    }
};
```

## Approach 4: Dynamic Programming with Space Optimization

### Solution
```cpp
// Time Complexity: O(m*n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int minPathSum(std::vector<std::vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        // Use a 1D dp array to save space
        std::vector<int> dp(n, 0);
        dp[0] = grid[0][0]; // Initialize the starting point
        
        // Fill the first row (can only come from the left)
        for (int i = 1; i < n; ++i) {
            dp[i] = dp[i - 1] + grid[0][i];
        }

        // Fill for each row using the dp array
        for (int i = 1; i < m; ++i) {
            dp[0] += grid[i][0]; // Update the first column
            for (int j = 1; j < n; ++j) {
                dp[j] = grid[i][j] + std::min(dp[j], dp[j - 1]); // Update by choosing minimum of top or left
            }
        }

        return dp[n - 1]; // Return the last cell which contains the result
    }
};
```

