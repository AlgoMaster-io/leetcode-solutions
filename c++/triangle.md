# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Tabulation (Bottom-Up Dynamic Programming)](#approach-3-tabulation-bottom-up-dynamic-programming)
- [Approach 4: Space Optimized Dynamic Programming](#approach-4-space-optimized-dynamic-programming)

---

## Approach 1: Recursive Approach

### Intuition:
The simplest way to solve this problem is through a recursive approach. Starting from the top of the triangle, for each element, we can choose to move either to the element just below it or the element below and to the right. The objective is to find the minimum path sum starting from the top and reaching any element at the bottom.

### Time and Space Complexity:
- **Time Complexity:** O(2^n), where n is the number of rows in the triangle. This is due to the overlapping subproblems in recursion.
- **Space Complexity:** O(n), which is the depth of the recursion stack.

```cpp
class Solution {
public:
    int minPathSumRecursive(vector<vector<int>>& triangle, int row, int col) {
        int n = triangle.size();
        // Base case: If we reached the last row, return the value of the element
        if (row == n - 1) {
            return triangle[row][col];
        }
        
        // Recur for the next row and adjacent columns
        int down = minPathSumRecursive(triangle, row + 1, col);
        int diagonal = minPathSumRecursive(triangle, row + 1, col + 1);
        
        // Return the current element plus the minimum path sum of its two possible paths
        return triangle[row][col] + min(down, diagonal);
    }
    
    int minimumTotal(vector<vector<int>>& triangle) {
        return minPathSumRecursive(triangle, 0, 0);
    }
};
```

---

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition:
The recursive approach has a lot of overlapping subproblems, which can be optimized using memoization. By storing the results of subproblems, we can avoid recalculating the same results multiple times and thereby reduce the time complexity.

### Time and Space Complexity:
- **Time Complexity:** O(n^2), where n is the number of rows in the triangle. Each element's subproblem is computed at most once.
- **Space Complexity:** O(n^2), required for the memoization table plus the recursion stack.

```cpp
class Solution {
public:
    int minPathSumMemoization(vector<vector<int>>& triangle, int row, int col, vector<vector<int>>& dp) {
        int n = triangle.size();
        // Base case: If we reached the last row, return the value of the element
        if (row == n - 1) {
            return triangle[row][col];
        }
        
        // Check memoization table
        if (dp[row][col] != -1) {
            return dp[row][col];
        }
        
        // Recur for the next row and adjacent columns and store the result in dp table
        int down = minPathSumMemoization(triangle, row + 1, col, dp);
        int diagonal = minPathSumMemoization(triangle, row + 1, col + 1, dp);
        
        // Memoize the result
        return dp[row][col] = triangle[row][col] + min(down, diagonal);
    }
    
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp(n, vector<int>(n, -1));
        return minPathSumMemoization(triangle, 0, 0, dp);
    }
};
```

---

## Approach 3: Tabulation (Bottom-Up Dynamic Programming)

### Intuition:
We can solve the problem using a bottom-up dynamic programming approach. Here, we start from the last row and move upwards. The idea is to convert the triangle into a dp table where each entry at (i, j) represents the minimum path sum from (i, j) to the bottom.

### Time and Space Complexity:
- **Time Complexity:** O(n^2), we visit each element once.
- **Space Complexity:** O(n^2), for the dp table.

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp = triangle; // Create a dp table initialized with triangle values
        
        // Start from the second last row and move upwards
        for (int row = n - 2; row >= 0; --row) {
            for (int col = 0; col <= row; ++col) {
                // Choose the minimum path sum of the two adjacent numbers in the row below
                dp[row][col] += min(dp[row + 1][col], dp[row + 1][col + 1]);
            }
        }
        
        // The answer is the minimum path sum starting from the top of the triangle
        return dp[0][0];
    }
};
```

---

## Approach 4: Space Optimized Dynamic Programming

### Intuition:
The tabulation approach uses O(n^2) space, but this can be optimized to O(n) by only keeping track of the current row and the row below it at any time. By doing this, we can improve the space efficiency of our solution.

### Time and Space Complexity:
- **Time Complexity:** O(n^2), similar to the tabulation method.
- **Space Complexity:** O(n), space reduced to keep two rows of the dp table only.

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<int> dp(triangle[n - 1]); // Start with the last row
        
        // Start from the second last row and move upwards
        for (int row = n - 2; row >= 0; --row) {
            for (int col = 0; col <= row; ++col) {
                // Update the current dp with the minimum sum path using values from the next row
                dp[col] = triangle[row][col] + min(dp[col], dp[col + 1]);
            }
        }
        
        // The answer is the minimum path sum starting from the top of the triangle
        return dp[0];
    }
};
```

In this final approach, space is significantly reduced, and the problem is solved efficiently using dynamic programming principles.

