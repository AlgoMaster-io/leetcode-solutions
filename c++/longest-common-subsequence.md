# [Leetcode 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming - Memoization Approach (Top-Down)](#dynamic-programming---memoization-approach-top-down)
- [Dynamic Programming - Tabulation Approach (Bottom-Up)](#dynamic-programming---tabulation-approach-bottom-up)
- [Dynamic Programming - Space Optimized Approach](#dynamic-programming---space-optimized-approach)

---

### Recursive Approach

The recursive approach is the most basic way to solve the Longest Common Subsequence problem. In this method, we start from the beginning of both strings and try to match characters. If a character matches, we increment our result for this match and move to the next pair of characters. If they do not match, we consider the maximum LCS without including either character by moving one character forward in each string separately.

#### Intuition
- If the characters match, include it in LCS and check the rest of the strings.
- If they do not match, try ignoring the current character from one of the strings and take the maximum LCS from the two possibilities.

#### Code
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        return lcsHelper(text1, text2, 0, 0);
    }
    
    int lcsHelper(string& text1, string& text2, int i, int j) {
        // Base case: If we reach the end of either string
        if (i == text1.size() || j == text2.size()) {
            return 0;
        }
        
        // If the current characters match, move both pointers
        if (text1[i] == text2[j]) {
            return 1 + lcsHelper(text1, text2, i + 1, j + 1);
        }
        
        // If they don't match, try advancing one of the pointers
        return max(lcsHelper(text1, text2, i + 1, j), lcsHelper(text1, text2, i, j + 1));
    }
};
```

#### Complexity Analysis
- **Time Complexity:** O(2^(m+n)), where m and n are the lengths of the two strings. This is because each call branches into two recursive calls.
- **Space Complexity:** O(m + n), for the recursion stack.

---

### Dynamic Programming - Memoization Approach (Top-Down)

The recursive approach can be inefficient due to repeated calculations. We can improve it using memoization by storing results of subproblems in a table.

#### Intuition
- Use a 2D table to store results of subproblems.
- The table entry `dp[i][j]` will store the length of the LCS of `text1[i...]` and `text2[j...]`.

#### Code
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> memo(text1.size(), vector<int>(text2.size(), -1));
        return lcsHelper(text1, text2, 0, 0, memo);
    }
    
    int lcsHelper(string& text1, string& text2, int i, int j, vector<vector<int>>& memo) {
        // Base case
        if (i == text1.size() || j == text2.size()) {
            return 0;
        }
        
        // Check if the result is already memoized
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        
        // Recursive case
        if (text1[i] == text2[j]) {
            memo[i][j] = 1 + lcsHelper(text1, text2, i + 1, j + 1, memo);
        } else {
            memo[i][j] = max(lcsHelper(text1, text2, i + 1, j, memo), lcsHelper(text1, text2, i, j + 1, memo));
        }
        
        return memo[i][j];
    }
};
```

#### Complexity Analysis
- **Time Complexity:** O(m * n), since we compute each state once and reuse the result.
- **Space Complexity:** O(m * n), due to the memoization table.

---

### Dynamic Programming - Tabulation Approach (Bottom-Up)

In this method, instead of recursive approach, we fill a 2D table iteratively. Each entry `dp[i][j]` represents the LCS of the substring `text1[..i-1]` and `text2[..j-1]`.

#### Intuition
- Build the solution of subproblems iteratively from smaller problems to bigger ones.
- The solution at each point depends on previously solved subproblems stored in the table.

#### Code
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        
        // Fill the dp table
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                // If characters match, 1 + previous diagonal value
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    // If they don't match, take max from left (i, j-1) or top (i-1, j)
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        // Return the value for the full strings
        return dp[m][n];
    }
};
```

#### Complexity Analysis
- **Time Complexity:** O(m * n), for filling up the table.
- **Space Complexity:** O(m * n), for the dp table.

---

### Dynamic Programming - Space Optimized Approach

The tabulation method uses a 2D table, but at any point, we only need the current row and the previous row for calculation. Thus, we can optimize space by maintaining only two 1D arrays.

#### Intuition
- Use two rows instead of a full table since each row only depends on the previous one.

#### Code
```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        vector<int> prev(n + 1, 0), current(n + 1, 0);
        
        // Fill the dp array row by row
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (text1[i - 1] == text2[j - 1]) {
                    current[j] = 1 + prev[j - 1];
                } else {
                    current[j] = max(prev[j], current[j - 1]);
                }
            }
            // Swap current and prev for the next iteration
            swap(prev, current);
        }
        
        // The answer is in the last computed row
        return prev[n];
    }
};
```

#### Complexity Analysis
- **Time Complexity:** O(m * n), for filling up the arrays.
- **Space Complexity:** O(n), reduced from the 2D table to just two rows. 

These approaches show how the problem can be tackled from a brute force method to a highly optimized dynamic programming technique, allowing understanding of both recursive and iterative methods with optimization through space reduction.

