# [Leetcode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Backtracking](#approach-2-backtracking)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

### Approach 1: Brute Force

#### Intuition
The simplest approach is to generate all possible strings of `2n` characters where each character is either '(' or ')'. For each generated string, we need to check if it forms a valid set of parentheses. Though simple, this approach is inefficient as it generates many incorrect strings that do not need to be checked.

#### Implementation

```cpp
#include <vector>
#include <string>

class Solution {
public:
    bool isValid(const std::string& str) {
        int balance = 0;
        for (char c : str) {
            balance += (c == '(') ? 1 : -1;
            if (balance < 0) return false; // More closing brackets
        }
        return balance == 0; // Equal count of '(' and ')'
    }

    void generateAll(std::string current, int n, std::vector<std::string>& result) {
        if (current.length() == 2 * n) { // Base case: string of length 2*n
            if (isValid(current)) {
                result.push_back(current); // Add valid string to result
            }
            return;
        }
        generateAll(current + "(", n, result); // Recur with added '('
        generateAll(current + ")", n, result); // Recur with added ')'
    }

    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::string> result;
        generateAll("", n, result); // Start with an empty string
        return result;
    }
};
```

#### Time Complexity
- **Time Complexity**: O(2^(2n) * n) - There are 2^(2n) strings of length 2n and we check each string for validity which takes O(n) time.
- **Space Complexity**: O(2^(2n) * n) - Space used to store all possible strings.

---

### Approach 2: Backtracking

#### Intuition
This method intelligently builds only valid sequences. We can add an opening bracket if it will not exceed `n`, and a closing bracket only if it will not exceed the number of opening brackets.

#### Implementation

```cpp
#include <vector>
#include <string>

class Solution {
public:
    void backtrack(std::string current, int open, int close, int max, std::vector<std::string>& result) {
        if (current.length() == max * 2) { // Base condition: max pair of parentheses
            result.push_back(current);
            return;
        }
        if (open < max) // Add '(' till it's less than max
            backtrack(current + "(", open + 1, close, max, result);
        if (close < open) // Add ')' only if it won't exceed '('
            backtrack(current + ")", open, close + 1, max, result);
    }

    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::string> result;
        backtrack("", 0, 0, n, result); // Start the backtracking process
        return result;
    }
};
```

#### Time Complexity
- **Time Complexity**: O(4^n / sqrt(n)) - Catalan number C(n) as only valid sequences are generated.
- **Space Complexity**: O(4^n / sqrt(n)) - Required space for the result storage.

---

### Approach 3: Dynamic Programming

#### Intuition
We construct valid parentheses by combining smaller valid parentheses pairs. For example, to get `f(3)`, where `f(n)` is all the valid parentheses of size `n`, we might prefix `f(2)` results inside `()`, and after the prefix, insert results of `f(1)`.

#### Implementation

```cpp
#include <vector>
#include <string>

class Solution {
public:
    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::vector<std::string>> dp(n + 1);
        dp[0].push_back(""); // Base case: empty string for 0 pairs

        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                for (const std::string& left : dp[j]) {
                    for (const std::string& right : dp[i - j - 1]) {
                        dp[i].push_back("(" + left + ")" + right);
                    }
                }
            }
        }
        return dp[n];
    }
};
```

#### Time Complexity
- **Time Complexity**: O(4^n / sqrt(n)) - There are C(n) distinct combinations of well-formed parentheses.
- **Space Complexity**: O(4^n / sqrt(n)) - Required space due to dynamic programming state storage for each subset.

