# [Leetcode 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Dynamic Programming - Bottom-Up](#approach-2-dynamic-programming---bottom-up)
- [Approach 3: Dynamic Programming - Optimized Space](#approach-3-dynamic-programming---optimized-space)

### Approach 1: Recursive Approach

**Intuition:**

The problem can be broken down by considering whether the current character from both `s1` and `s2` can match the current character of `s3`. If a character from `s1` matches, we move to the next character in `s1` and `s3`, similarly for `s2`. This process continues recursively.

**Code:**

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        // Base condition, check if the lengths add up
        if (s1.size() + s2.size() != s3.size()) {
            return false;
        }
        return helper(s1, s2, s3, 0, 0, 0);
    }

    // Helper function for recursion
    bool helper(string& s1, string& s2, string& s3, int i, int j, int k) {
        // If we have considered all characters of s3
        if (k == s3.size()) {
            return true;
        }

        // Check if we can take a character from s1
        if (i < s1.size() && s1[i] == s3[k]) {
            if (helper(s1, s2, s3, i + 1, j, k + 1)) {
                return true;
            }
        }

        // Check if we can take a character from s2
        if (j < s2.size() && s2[j] == s3[k]) {
            if (helper(s1, s2, s3, i, j + 1, k + 1)) {
                return true;
            }
        }

        return false;
    }
};
```

**Complexity:**

- **Time complexity:** O(2^(m+n)), where m is the length of s1 and n is the length of s2, due to the exponential combination of tasks at each step.
- **Space complexity:** O(m+n) for the recursion stack.

### Approach 2: Dynamic Programming - Bottom-Up

**Intuition:**

We use a DP table where `dp[i][j]` means the first `i` characters of `s1` and the first `j` characters of `s2` can form the first `i+j` characters of `s3`. Transition through the DP table either by taking a character from `s1` or from `s2` and check both scenarios iteratively.

**Code:**

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size();
        if (m + n != s3.size()) return false;

        // Create a DP table to store results of subproblems
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        
        // Base case: both s1 and s2 are empty
        dp[0][0] = true;

        // Initialize the edges of the table
        for (int i = 1; i <= m; ++i) {
            dp[i][0] = dp[i-1][0] && s1[i-1] == s3[i-1];
        }
        for (int j = 1; j <= n; ++j) {
            dp[0][j] = dp[0][j-1] && s2[j-1] == s3[j-1];
        }

        // Fill the DP table
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                // Case 1: Match character from s1
                if (s1[i-1] == s3[i+j-1]) {
                    dp[i][j] = dp[i][j] || dp[i-1][j];
                }
                // Case 2: Match character from s2
                if (s2[j-1] == s3[i+j-1]) {
                    dp[i][j] = dp[i][j] || dp[i][j-1];
                }
            }
        }
        
        return dp[m][n];
    }
};
```

**Complexity:**

- **Time complexity:** O(m*n), as we fill the DP table of size m*n.
- **Space complexity:** O(m*n), for the DP table.

### Approach 3: Dynamic Programming - Optimized Space

**Intuition:**

Since each state in the DP table depends only on the previous states in the current and previous rows, we can optimize the space to use only O(n) by maintaining a single row and updating it in-place.

**Code:**

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size();
        if (m + n != s3.size()) return false;

        vector<bool> dp(n + 1, false);
        
        dp[0] = true;

        for (int j = 1; j <= n; ++j) {
            dp[j] = dp[j-1] && s2[j-1] == s3[j-1];
        }

        for (int i = 1; i <= m; ++i) {
            dp[0] = dp[0] && s1[i-1] == s3[i-1];
            for (int j = 1; j <= n; ++j) {
                dp[j] = (dp[j] && s1[i-1] == s3[i+j-1]) || (dp[j-1] && s2[j-1] == s3[i+j-1]);
            }
        }
        
        return dp[n];
    }
};
```

**Complexity:**

- **Time complexity:** O(m*n), similar to the full DP solution.
- **Space complexity:** O(n), since we only store the current row of the DP array.

