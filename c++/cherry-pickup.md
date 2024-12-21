# [Leetcode 741: Cherry Pickup](https://leetcode.com/problems/cherry-pickup/)

## Approaches

- [Approach 1: Dynamic Programming - Top Down with Memoization](#approach-1-dynamic-programming---top-down-with-memoization)
- [Approach 2: Dynamic Programming - Bottom Up](#approach-2-dynamic-programming---bottom-up)

---

## Approach 1: Dynamic Programming - Top Down with Memoization

### Intuition

The problem resembles a two-person game where two traversals need to be coordinated to maximize collected cherries, first while going downwards and then while coming back up. However, we can visualize it as paths from top-left to bottom-right and then again from bottom-right back to top-left. By reversing the second journey, it changes into a problem finding two paths from top-left to bottom-right simultaneously.

The key insight is that if two people start together from the start and always take the top-down path, then their respective returns would logically map the upward path. 

To solve this:

- Two people start at (0, 0) and both need to reach (n-1, n-1).
- We maintain a 3D dp array `dp[r1][c1][c2]` which records the maximum cherries picked where person A is at (r1, c1) and person B is at (r1 + c1 - c2, c2). Since the sum of rows + cols is always equal in a valid path, person B's row can be derived from other indices which economizes dimensions.

### Steps

1. Initialize a dp 3D vector and set default values to invalid cherry numbers to flag non-valid or non-beneficial paths.
2. Calculate recursively the maximum cherries collected for every valid state (r1, c1, c2) using combinatorial move choices.
3. Consider two cases for each position:
   - If the two people land at the same cell, collect cherries once.
   - Otherwise, cherries are added from both cells.
4. Use memoization to cache results for each state.

### Code

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

class Solution {
public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<vector<int>>> memo(n, vector<vector<int>>(n, vector<int>(n, INT_MIN)));
        return max(0, dp(grid, memo, 0, 0, 0, n));  // Starting from (0,0) for both
    }
    
private:
    int dp(vector<vector<int>>& grid, vector<vector<vector<int>>>& memo, int r1, int c1, int c2, int n) {
        int r2 = r1 + c1 - c2;
        
        if (r1 >= n || c1 >= n || r2 >= n || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1) {
            return INT_MIN;  // Base case: out of bounds or on a thorn
        }
        
        if (r1 == n - 1 && c1 == n - 1) {  // All cherries collected from top-left to bottom-right
            return grid[r1][c1];
        }
        
        if (memo[r1][c1][c2] != INT_MIN) {
            return memo[r1][c1][c2];
        }
        
        int cherries = grid[r1][c1];
        if (c1 != c2) {
            cherries += grid[r2][c2];
        }
        
        cherries += max({dp(grid, memo, r1 + 1, c1, c2, n),
                         dp(grid, memo, r1, c1 + 1, c2, n),
                         dp(grid, memo, r1 + 1, c1, c2 + 1, n),
                         dp(grid, memo, r1, c1 + 1, c2 + 1, n)});
        
        memo[r1][c1][c2] = cherries;
        return cherries;
    }
};
```

### Complexity

- **Time Complexity:** \(O(n^3)\) since we have a three-dimensional memoization matrix.
- **Space Complexity:** \(O(n^3)\) for the storage of memoized values.

---

## Approach 2: Dynamic Programming - Bottom Up

### Intuition

Instead of exploring top-down which usually introduces dynamic choices and requires careful handling of recursive states justified through memoization, a bottom-up construction can be undertaken.

We notice that, both persons start and finish at same locations, hence at `t` steps from start, there will be `t+1` number of possible locations on the grid for each person. This creates a practical boundary on movement which allows use of table-based DP.

Considering this bounded movement by step, we can calculate and maintain maximum cherries collected between valid `r1, c1` choices leading `r2`, `c2` as a decision-making strategy for all scenarios.

### Code

```cpp
class Solution {
public:
    int cherryPickup(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<vector<int>>> dp(2*n-1, vector<vector<int>>(n, vector<int>(n, INT_MIN)));
        dp[0][0][0] = grid[0][0];
        
        for (int t = 1; t < 2 * n - 1; ++t) {
            for (int r1 = std::min(n - 1, t); r1 >= 0; --r1) {
                for (int r2 = std::min(n - 1, t); r2 >= 0; --r2) {
                    int c1 = t - r1, c2 = t - r2;
                    if (c1 < 0 || c1 >= n || c2 < 0 || c2 >= n || grid[r1][c1] == -1 || grid[r2][c2] == -1) {
                        continue;
                    }
                    
                    int cherries = grid[r1][c1];
                    if (r1 != r2) {
                        cherries += grid[r2][c2];
                    }
                    
                    int max_prev = INT_MIN;
                    if (r1 > 0 && r2 > 0) max_prev = max(max_prev, dp[t-1][r1-1][r2-1]);
                    if (r1 > 0 && c2 > 0) max_prev = max(max_prev, dp[t-1][r1-1][r2]);
                    if (c1 > 0 && r2 > 0) max_prev = max(max_prev, dp[t-1][r1][r2-1]);
                    if (c1 > 0 && c2 > 0) max_prev = max(max_prev, dp[t-1][r1][r2]);
                    dp[t][r1][r2] = max_prev + cherries;
                }
            }
        }
        
        return max(0, dp[2*n-2][n-1][n-1]);
    }
};
```

### Complexity

- **Time Complexity:** \(O(n^3)\) as we must evaluate and fill each possible matrix state on a step-wise basis.
- **Space Complexity:** \(O(n^2)\) using prior reduction of DP dimensions by aggregation of results at each step t.

By employing these outlined strategies, we effectively establish and optimize a solved path for cherry collection consistency, rigorously using dynamic programming.

