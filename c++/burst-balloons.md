# [Leetcode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

## Approaches

- [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)
- [Dynamic Programming with Iterative Table Filling](#dynamic-programming-with-iterative-table-filling)

### Dynamic Programming with Memoization

**Intuition:**

In this approach, we explore the idea of bursting balloons in such a manner that we maximize the coins collected. We treat each balloon to be burst as a potential "last balloon" to maximize local profit. We then calculate profits of sub-problems recursively, memoizing solutions to avoid repetitive calculations.

1. Given balloons array `nums`, assume it is padded with `1`s at both ends. This assists in handling edge cases.
2. Define a DP function `dp(left, right)` that returns the maximum coins collected for balloons in the range `(left+1, right-1)`.
3. For a given balloon `i`, calculate coins gained if it were the last to burst in its range `(left, right)`, incorporating previously solved subproblems.
4. Use a memoization table to store results of subproblems for reuse.

**Code:**

```cpp
class Solution {
public:
    int dp(vector<int>& nums, vector<vector<int>>& memo, int left, int right) {
        // If the subproblem is already solved, return the solution
        if (left + 1 == right) return 0;
        if (memo[left][right] != -1) return memo[left][right];

        // Try bursting each balloon between left and right
        int maxCoins = 0;
        for (int i = left + 1; i < right; ++i) {
            // Coins collected from this particular choice
            int coins = nums[left] * nums[i] * nums[right];
            // Recursively compute the maximum coins by bursting sub-problems
            coins += dp(nums, memo, left, i) + dp(nums, memo, i, right);
            // Update the maximum
            maxCoins = max(maxCoins, coins);
        }
        // Save the computed result
        memo[left][right] = maxCoins;
        return maxCoins;
    }
    
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        vector<vector<int>> memo(n + 2, vector<int>(n + 2, -1));
        return dp(nums, memo, 0, n + 1);
    }
};
```

**Time Complexity:** O(n^3) — For each pair `(left, right)`, we can iterate over possible `i`s in `(left,right)`.  
**Space Complexity:** O(n^2) — We use a matrix to store the results of subproblems.

### Dynamic Programming with Iterative Table Filling

**Intuition:**

This approach is similarly grounded in dynamic programming, but uses an iterative approach to fill up a table. We iteratively solve sub-problems of increasing size. This improves potential overhead by avoiding recursive calls and stack management.

1. Create a 3D DP table `dp[left][right]` representing the maximum coins for the subproblem `(left+1, right-1)`.
2. Consider increasing subproblem sizes.
3. Fill the table by iterating through `left` and `right`, computing possible coin gains for each potential last burst `i`.
4. Use the results from smaller subproblems to build-up to the full problem solution.

**Code:**

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));

        // Solve subproblems of increasing size
        for (int length = 2; length <= n + 1; ++length) {
            for (int left = 0; left <= n + 1 - length; ++left) {
                int right = left + length;
                // Determine the best outcome for current subproblem
                for (int i = left + 1; i < right; ++i) {
                    dp[left][right] = max(dp[left][right], 
                                          nums[left] * nums[i] * nums[right] 
                                          + dp[left][i] + dp[i][right]);
                }
            }
        }
        
        return dp[0][n + 1];
    }
};
```

**Time Complexity:** O(n^3) — We iterate through all subproblem pairs `(left, right)` and consider all possibilities of `i`.  
**Space Complexity:** O(n^2) — The DP matrix `dp[left][right]` requires this space to store solutions of subproblems.

