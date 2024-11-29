# 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approach 1: Recursive Solution with Memoization

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestPalindromeSubseq(const std::string& s) {
        int n = s.size();
        std::vector<std::vector<int>> memo(n, std::vector<int>(n, 0));
        return longestPalindromeSubseqHelper(s, 0, n - 1, memo);
    }

private:
    int longestPalindromeSubseqHelper(const std::string& s, int start, int end, std::vector<std::vector<int>>& memo) {
        // Base case: single character is a palindrome
        if (start == end) {
            return 1;
        }
        // Base case: no characters (invalid scenario)
        if (start > end) {
            return 0;
        }
        // Check memo
        if (memo[start][end] != 0) {
            return memo[start][end];
        }
        // If characters match, continue inward
        if (s[start] == s[end]) {
            memo[start][end] = 2 + longestPalindromeSubseqHelper(s, start + 1, end - 1, memo);
        } else {
            // Explore both options and take the max
            memo[start][end] = std::max(longestPalindromeSubseqHelper(s, start + 1, end, memo),
                                        longestPalindromeSubseqHelper(s, start, end - 1, memo));
        }
        return memo[start][end];
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestPalindromeSubseq(const std::string& s) {
        int n = s.size();
        std::vector<std::vector<int>> dp(n, std::vector<int>(n, 0));

        // Every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // Check subproblems of increasing lengths
        for (int length = 2; length <= n; length++) {
            for (int start = 0; start <= n - length; start++) {
                int end = start + length - 1;
                // Characters match, add 2 + result for inner substring
                if (s[start] == s[end]) {
                    dp[start][end] = dp[start + 1][end - 1] + 2;
                } else {
                    // Choose the best option from excluding either end
                    dp[start][end] = std::max(dp[start + 1][end], dp[start][end - 1]);
                }
            }
        }

        // Result is in dp[0][n-1]
        return dp[0][n - 1];
    }
};
```

## Approach 3: Optimized Dynamic Programming with Reduced Space

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestPalindromeSubseq(const std::string& s) {
        int n = s.size();
        std::vector<int> dp(n, 0);
        std::vector<int> prevDp(n, 0);

        // Every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
        }

        // Check subproblems of increasing lengths
        for (int length = 2; length <= n; length++) {
            for (int start = 0; start <= n - length; start++) {
                int end = start + length - 1;
                int temp = dp[start]; // Preserve current value before overwriting

                if (s[start] == s[end]) {
                    dp[start] = prevDp[start + 1] + 2;
                } else {
                    dp[start] = std::max(dp[start + 1], prevDp[start]);
                }

                prevDp[start] = temp; // Update prevDp for the next iteration
            }
        }

        // Result is in dp[0]
        return dp[0];
    }
};
```

