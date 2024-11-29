# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
#include <vector>
#include <string>

class Solution {
public:
    int numDistinct(std::string s, std::string t) {
        // Memoization table
        std::vector<std::vector<int>> memo(s.length(), std::vector<int>(t.length(), -1));
        return numDistinctHelper(s, t, 0, 0, memo);
    }
    
private:
    int numDistinctHelper(const std::string& s, const std::string& t, int sIndex, int tIndex, std::vector<std::vector<int>>& memo) {
        // If t is exhausted, one subsequence is found
        if (tIndex == t.length()) {
            return 1;
        }
        // If s is exhausted first, no subsequence can be formed
        if (sIndex == s.length()) {
            return 0;
        }
        // Check memoization table
        if (memo[sIndex][tIndex] != -1) {
            return memo[sIndex][tIndex];
        }
        
        // If characters match, both decisions can be made
        if (s[sIndex] == t[tIndex]) {
            memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex + 1, tIndex + 1, memo) + 
                                   numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        } else {
            // If characters don't match, skip the current character in s
            memo[sIndex][tIndex] = numDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        }
        
        return memo[sIndex][tIndex];
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
#include <vector>
#include <string>

class Solution {
public:
    int numDistinct(std::string s, std::string t) {
        // DP table
        std::vector<std::vector<int>> dp(s.length() + 1, std::vector<int>(t.length() + 1, 0));

        // Base case: empty t can be formed by all possible substrings of s
        for (int i = 0; i <= s.length(); i++) {
            dp[i][t.length()] = 1;
        }

        // Fill the table in reverse order
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = t.length() - 1; j >= 0; j--) {
                if (s[i] == t[j]) {
                    // Both using the character and ignoring it
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
                } else {
                    // Ignore the character in s
                    dp[i][j] = dp[i + 1][j];
                }
            }
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return dp[0][0];
    }
};
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(m)
#include <vector>
#include <string>

class Solution {
public:
    int numDistinct(std::string s, std::string t) {
        // Previous and current row for space optimization
        std::vector<int> prev(t.length() + 1, 0);
        std::vector<int> curr(t.length() + 1, 0);
        
        // Base case: empty t can be formed by all possible substrings of s
        for (int i = 0; i <= s.length(); i++) {
            prev[t.length()] = 1;
        }
        
        // Fill the table in reverse order
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = t.length() - 1; j >= 0; j--) {
                if (s[i] == t[j]) {
                    curr[j] = prev[j + 1] + prev[j];
                } else {
                    curr[j] = prev[j];
                }
            }
            // Move current row to previous for next iteration
            prev = curr;
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return prev[0];
    }
};
```

