# 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach 1: Brute Force (Generate All Combinations)

### Solution
```cpp
// Time Complexity: O(2^(2n) * n), where 2^(2n) is the number of combinations and n is the time to validate each combination
// Space Complexity: O(2^(2n) * n) for storing the combinations
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::string> result;
        std::string current(2 * n, ' ');
        generateAll(current, 0, result);
        return result;
    }

private:
    void generateAll(std::string& current, int pos, std::vector<std::string>& result) {
        if (pos == current.length()) {
            if (isValid(current)) {
                result.push_back(current); // Add valid combination to result
            }
            return;
        }

        current[pos] = '('; // Place an open parenthesis
        generateAll(current, pos + 1, result);
        current[pos] = ')'; // Place a close parenthesis
        generateAll(current, pos + 1, result);
    }

    bool isValid(const std::string& current) {
        int balance = 0;
        for (char c : current) {
            if (c == '(') {
                balance++;
            } else {
                balance--;
            }
            if (balance < 0) {
                return false; // Invalid if closing parenthesis exceeds opening
            }
        }
        return balance == 0; // Valid if balance is zero
    }
};
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::string> result;
        backtrack(result, "", 0, 0, n);
        return result;
    }

private:
    void backtrack(std::vector<std::string>& result, std::string current, int open, int close, int max) {
        if (current.length() == max * 2) {
            result.push_back(current); // Add valid combination to result
            return;
        }

        if (open < max) {
            backtrack(result, current + '(', open + 1, close, max); // Add an open parenthesis
        }
        if (close < open) {
            backtrack(result, current + ')', open, close + 1, max); // Add a close parenthesis
        }
    }
};
```

## Approach 3: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::vector<std::string>> dp(n + 1);
        dp[0].push_back("");

        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                for (const std::string& left : dp[j]) {
                    for (const std::string& right : dp[i - 1 - j]) {
                        dp[i].push_back("(" + left + ")" + right); // Generate combinations
                    }
                }
            }
        }

        return dp[n];
    }
};
```

