# [Leetcode 150: Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches:
1. [Basic Approach: Brute Force Using Stack](#basic-approach-brute-force-using-stack)
2. [Optimized Approach](#optimized-approach)

### Basic Approach: Brute Force Using Stack

**Intuition:**

The Reverse Polish Notation (RPN) is a mathematical notation in which every operator follows all of its operands. The simplest way to evaluate such an expression is by using a stack. The stack data structure allows us to process the operands sequentially and apply operators to the numbers popped from the stack.

**Approach:**

1. Initialize an empty stack.
2. Iterate over each token in the given list of tokens.
3. If the token is an operand (number), push it onto the stack.
4. If the token is an operator (+, -, *, /), pop the top two numbers from the stack, apply the operator, and push the result back onto the stack.
5. After processing all tokens, the result of the expression will be the only number left in the stack.

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <string>

class Solution {
public:
    int evalRPN(std::vector<std::string>& tokens) {
        std::stack<int> st;
        
        for (const std::string& token : tokens) {
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                // Pop two top operands from the stack for the operation
                int num2 = st.top();
                st.pop();
                int num1 = st.top();
                st.pop();
                
                if (token == "+") {
                    st.push(num1 + num2);
                } else if (token == "-") {
                    st.push(num1 - num2);
                } else if (token == "*") {
                    st.push(num1 * num2);
                } else if (token == "/") {
                    st.push(num1 / num2); // note: integer division
                }
            } else {
                // Convert token to integer and push onto stack
                st.push(std::stoi(token));
            }
        }
        
        // The result of the RPN expression is the last remaining item in the stack
        return st.top();
    }
};
```

**Time Complexity:** O(n), where n is the number of tokens. We process each token exactly once.

**Space Complexity:** O(n), in the worst case, if all tokens are numbers, they all need to be pushed onto the stack.

### Optimized Approach

Since the basic approach is already optimal due to its O(n) time complexity, there's essentially no more "optimized" approach with a better time complexity for this problem than using a stack because the problem inherently depends on sequential token processing.

The space can also not be reduced significantly since having a different approach that doesn't utilize auxiliary space like a stack would be complex and typically less efficient or feasible.

Hence, the approach with a stack remains the best trade-off regarding maintainability and performance for this problem.

