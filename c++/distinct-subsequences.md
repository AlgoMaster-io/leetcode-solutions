# [Leetcode 115: Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization (Top-Down Dynamic Programming)](#approach-2-recursion-with-memoization-top-down-dynamic-programming)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)

## Approach 1: Recursion

### Intuition
The fundamental idea is to solve this problem through a top-down recursive approach. At each step, we decide whether to match the current character in `s` with the current character in `t`. If they match, we have two choices: use this character in `s` to form a subsequence or skip it in `s`. If they don’t match, we can only skip the current character in `s`.

### Algorithm
1. Use a recursive function that takes indices `i` and `j` representing current characters in strings `s` and `t`.
2. If `j` reaches the end of `t`, a subsequence is found, so return 1.
3. If `i` reaches the end of `s`, `t` can't be matched any further, so return 0.
4. Recursive Case:
   - If `s[i] == t[j]`, then we have two choices:
     - Match `s[i]` with `t[j]` and recurse for the remaining strings.
     - Skip `s[i]` and recursively find further matches.
   - If `s[i] != t[j]`, recursively skip `s[i]`.
5. Sum up the results of both options.

### Code
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        return countSubsequences(s, t, 0, 0);
    }
    
private:
    int countSubsequences(const string &s, const string &t, int i, int j) {
        if (j == t.size()) return 1; // t is completely matched
        if (i == s.size()) return 0; // s is exhausted, and t is not matched

        if (s[i] == t[j]) {
            // Match current characters and move both indices forward
            // Or skip the current character of s
            return countSubsequences(s, t, i + 1, j + 1) + countSubsequences(s, t, i + 1, j);
        } else {
            // Skip the current character of s
            return countSubsequences(s, t, i + 1, j);
        }
    }
};
```

### Complexity
- **Time Complexity**: O(2^n), where n is the length of `s`, because each character in `s` can be chosen or not.
- **Space Complexity**: O(n) for the recursion stack.

## Approach 2: Recursion with Memoization (Top-Down Dynamic Programming)

### Intuition
To reduce repeated computation seen in the basic recursive approach, we employ memoization. We store the results of previous computations in a 2D array (or map), where each entry represents the result of solving the subproblem with indices `i` and `j`.

### Algorithm
1. Use a 2D vector `dp` initialized to -1 to store intermediate results.
2. Follow the same recursive structure as above, but first check if `dp[i][j]` has been computed.
3. If already computed, return this value.
4. Otherwise, compute it and store it in `dp[i][j]`.
5. Return the value stored in `dp[i][j]`.

### Code
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<int>> dp(s.size(), vector<int>(t.size(), -1));
        return countSubsequences(s, t, 0, 0, dp);
    }
    
private:
    int countSubsequences(const string &s, const string &t, int i, int j, vector<vector<int>>& dp) {
        if (j == t.size()) return 1; // t is completely matched
        if (i == s.size()) return 0; // s is exhausted, and t is not matched

        if (dp[i][j] != -1) return dp[i][j]; // Return cached result

        if (s[i] == t[j]) {
            dp[i][j] = countSubsequences(s, t, i + 1, j + 1, dp) + countSubsequences(s, t, i + 1, j, dp);
        } else {
            dp[i][j] = countSubsequences(s, t, i + 1, j, dp);
        }
        return dp[i][j];
    }
};
```

### Complexity
- **Time Complexity**: O(n * m), where n is the length of `s` and m is the length of `t`.
- **Space Complexity**: O(n * m) for the memoization table and O(n) for recursion stack depth.

## Approach 3: Dynamic Programming (Bottom-Up)

### Intuition
In the bottom-up dynamic programming approach, we use a table to iteratively build up solutions to subproblems from smaller subproblems to larger subproblems. This eliminates the overhead of recursion and reduces space complexity by explicitly managing the table.

### Algorithm
1. Create a 2D DP table `dp` where `dp[i][j]` represents the number of distinct subsequences of `s[i:]` that equals `t[j:]`.
2. Initialize `dp[i][m]` to 1 for all `i` as an empty `t` is a subsequence of any prefix of `s`.
3. Initialize `dp[n][j]` to 0 for all `j > 0` as non-empty `t` cannot match an empty `s`.
4. Iterate backwards over `i` and `j` to fill this table.
5. If `s[i] == t[j]`, then `dp[i][j] = dp[i+1][j+1] + dp[i+1][j]`.
6. Otherwise, `dp[i][j] = dp[i+1][j]`.
7. The result resides in `dp[0][0]`.

### Code
```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size(), m = t.size();
        vector<vector<long>> dp(n + 1, vector<long>(m + 1, 0));

        // Initialize the last row: If `t` is empty, there is one subsequence of any prefix of `s`.
        for (int i = 0; i <= n; ++i) {
            dp[i][m] = 1;
        }

        // Fill the dynamic programming table
        for (int i = n - 1; i >= 0; --i) {
            for (int j = m - 1; j >= 0; --j) {
                if (s[i] == t[j]) {
                    // Add both options: use s[i] or skip s[i]
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                } else {
                    // Skip s[i]
                    dp[i][j] = dp[i + 1][j];
                }
            }
        }
        
        return dp[0][0];
    }
};
```

### Complexity
- **Time Complexity**: O(n * m)
- **Space Complexity**: O(n * m), though it can be optimized to O(m) using a rolling array technique.

This suite of solutions provides a range of strategies from a basic recursive approach to a more complex dynamic programming method, each progressively optimizing for time and space usage.

