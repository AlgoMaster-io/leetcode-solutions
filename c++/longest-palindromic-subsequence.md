# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)

---

## Approach 1: Recursive Approach

### Intuition
The recursive approach will help understand the problem depth-first. The idea is to find the longest palindromic subsequence by recursively checking pairs of characters from the start and the end of the string. If they match, move inward, otherwise try both possibilities by ignoring one of the characters either from the start or the end.

### Steps:
- If the start is greater than the end, return 0 (base case).
- If the start equals end, return 1 as a single character is a palindrome.
- If the start and end characters are equal, contribute these two characters to the palindromic subsequence and recurse inward.
- Otherwise, take the maximum of two choices: Ignore the start character or ignore the end character.

```cpp
class Solution {
public:
    int longestPalindromeSubseq(std::string s) {
        return recursiveHelper(s, 0, s.length() - 1);
    }

private:
    int recursiveHelper(const std::string &s, int start, int end) {
        if (start > end) return 0;
        if (start == end) return 1;
        
        if (s[start] == s[end]) {
            return 2 + recursiveHelper(s, start + 1, end - 1);
        } else {
            return std::max(recursiveHelper(s, start + 1, end), recursiveHelper(s, start, end - 1));
        }
    }
};
```

### Time Complexity
- **Time**: O(2^n) in the worst case due to the two recursive calls for non-palindrome matched character pair, where `n` is the length of string `s`.
- **Space**: O(n) due to the recursive call stack in the worst case.

---

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition
The recursive approach has overlapping subproblems, which can be optimized using memoization. Instead of recalculating the solutions for overlapping subproblems, store and reuse them.

### Steps:
- Use a 2D vector to store results of subproblems defined by start and end indices.
- Apply the same recursive logic as in the first approach.
- Store results of subproblems in the memo table to avoid recalculations.

```cpp
class Solution {
public:
    int longestPalindromeSubseq(std::string s) {
        int n = s.length();
        std::vector<std::vector<int>> memo(n, std::vector<int>(n, -1));
        return memoizationHelper(s, 0, n - 1, memo);
    }

private:
    int memoizationHelper(const std::string &s, int start, int end, std::vector<std::vector<int>> &memo) {
        if (start > end) return 0;
        if (start == end) return 1;
        if (memo[start][end] != -1) return memo[start][end];
        
        if (s[start] == s[end]) {
            memo[start][end] = 2 + memoizationHelper(s, start + 1, end - 1, memo);
        } else {
            memo[start][end] = std::max(memoizationHelper(s, start + 1, end, memo), memoizationHelper(s, start, end - 1, memo));
        }
        return memo[start][end];
    }
};
```

### Time Complexity
- **Time**: O(n^2) because we compute the result for each subproblem exactly once.
- **Space**: O(n^2) for the memoization table plus O(n) due to recursive stack.

---

## Approach 3: Dynamic Programming (Bottom-Up)

### Intuition
Using a bottom-up dynamic programming approach, we build the solution for every subsequence of the string. This eliminates the function call overhead of recursion and handles larger inputs efficiently.

### Steps:
- Create a 2D vector `dp` where `dp[i][j]` represents the length of the longest palindromic subsequence between indices `i` and `j`.
- Initialize `dp[i][i]` to 1, as one character is a palindrome of length 1.
- Iterate over substring lengths from 2 to `n`. For each length, iterate over potential start indices and determine the end indices.
- Use previous results to compute each `dp[i][j]`.

```cpp
class Solution {
public:
    int longestPalindromeSubseq(std::string s) {
        int n = s.length();
        std::vector<std::vector<int>> dp(n, std::vector<int>(n, 0));

        for (int i = 0; i < n; ++i) {
            dp[i][i] = 1;
        }

        for (int len = 2; len <= n; ++len) {
            for (int i = 0; i <= n - len; ++i) {
                int j = i + len - 1;
                if (s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = std::max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][n - 1];
    }
};
```

### Time Complexity
- **Time**: O(n^2) using nested loops over possible substrings.
- **Space**: O(n^2) for storing dp results.

This dynamic programming solution is optimal for the problem in terms of practical implementation and efficiency.

