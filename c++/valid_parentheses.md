# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <stack>
#include <string>

class Solution {
public:
    bool isValid(std::string s) {
        std::stack<char> stack;

        // Iterate through the string
        for (char c : s) {
            // Push opening brackets onto the stack
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            }
            // Check closing brackets
            else {
                // If stack is empty or doesn't match the top, return false
                if (stack.empty() || !isMatchingPair(stack.top(), c)) {
                    return false;
                }
                stack.pop();
            }
        }

        // If the stack is empty, all brackets are matched
        return stack.empty();
    }

private:
    bool isMatchingPair(char open, char close) {
        return (open == '(' && close == ')') ||
               (open == '{' && close == '}') ||
               (open == '[' && close == ']');
    }
};
```

