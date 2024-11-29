# 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approach 1: Recursive Approach

### Solution
```cpp
// Time Complexity: Exponential
// Space Complexity: O(m + n) for the recursion stack
#include <string>
#include <algorithm>

class Solution {
public:
    int longestCommonSubsequence(const std::string& text1, const std::string& text2) {
        return lcs(text1, text2, text1.length(), text2.length());
    }
    
private:
    int lcs(const std::string& s1, const std::string& s2, int m, int n) {
        // Base case: If either string is empty, return 0
        if (m == 0 || n == 0) {
            return 0;
        }
        
        // If the last characters match, reduce both lengths and add 1 to the result
        if (s1[m - 1] == s2[n - 1]) {
            return 1 + lcs(s1, s2, m - 1, n - 1);
        } else {
            // Otherwise, take the maximum result by reducing one string at a time
            return std::max(lcs(s1, s2, m - 1, n), lcs(s1, s2, m, n - 1));
        }
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestCommonSubsequence(const std::string& text1, const std::string& text2) {
        int m = text1.length();
        int n = text2.length();
        std::vector<std::vector<int>> dp(m + 1, std::vector<int>(n + 1, 0));
        
        // Fill the DP table iteratively
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    // Matching characters, take diagonal value and add 1
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    // Different characters, take the maximum of left or top cell
                    dp[i][j] = std::max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        // The bottom-right cell contains the length of LCS
        return dp[m][n];
    }
};
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(min(m, n))
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int longestCommonSubsequence(const std::string& text1, const std::string& text2) {
        if (text1.length() < text2.length()) {
            return longestCommonSubsequence(text2, text1); // Ensure text1 is larger or equal
        }
        
        std::vector<int> prev(text2.length() + 1, 0);

        // Iterate through each character of text1
        for (int i = 1; i <= text1.length(); i++) {
            std::vector<int> curr(text2.length() + 1, 0);

            // Iterate through each character of text2
            for (int j = 1; j <= text2.length(); j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    curr[j] = 1 + prev[j - 1];
                } else {
                    curr[j] = std::max(curr[j - 1], prev[j]);
                }
            }
            prev = curr; // Move current row to previous for the next iteration
        }

        return prev[text2.length()]; // Last computed row contains the result
    }
};
```

