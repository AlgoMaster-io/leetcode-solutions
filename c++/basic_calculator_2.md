# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <stack>
#include <string>
using namespace std;

class Solution {
public:
    int calculate(string s) {
        stack<int> stack;
        int currentNumber = 0;
        char operation = '+'; // Default operation is addition

        for (int i = 0; i < s.length(); i++) {
            char c = s[i];

            // Build the current number
            if (isdigit(c)) {
                currentNumber = currentNumber * 10 + (c - '0');
            }

            // Process operators and the last number
            if (!isdigit(c) && c != ' ' || i == s.length() - 1) {
                if (operation == '+') {
                    stack.push(currentNumber);
                } else if (operation == '-') {
                    stack.push(-currentNumber);
                } else if (operation == '*') {
                    int top = stack.top(); stack.pop();
                    stack.push(top * currentNumber);
                } else if (operation == '/') {
                    int top = stack.top(); stack.pop();
                    stack.push(top / currentNumber);
                }

                operation = c; // Update the operation
                currentNumber = 0; // Reset the current number
            }
        }

        // Sum up all the values in the stack
        int result = 0;
        while (!stack.empty()) {
            result += stack.top();
            stack.pop();
        }

        return result;
    }
};
```

