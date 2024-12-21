# [Leetcode 227: Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approaches

1. [Basic Approach: Using two stacks](#approach-1-basic-approach-using-two-stacks)
2. [Optimized Approach: Using a single stack](#approach-2-optimized-approach-using-a-single-stack)

### Approach 1: Basic Approach: Using two stacks

#### Intuition:
In this approach, we can utilize two stacks: one for numbers and one for operators. The idea is to iterate over the string character by character and process each number or operator. The priority of multiplication and division over addition and subtraction requires managing the operations according to their precedence, which can be intuitively handled using stacks.

#### Steps:
1. Create two stacks: one for numbers and one for operators.
2. Iterate over the expression:
   - If the character is a digit, continue building the number.
   - If the character is an operator, process the previously formed number, and check for operator precedence to determine if any operations in the stack need to be performed.
   - Push the operator and the current number onto their respective stacks.
3. After processing the entire string, handle any remaining operations in the stack.
4. Calculate the result from the stacks by executing stack operations in the right order.

#### Code:

```cpp
#include <iostream>
#include <stack>
#include <string>

int calculate(std::string s) {
    std::stack<int> numbers;
    std::stack<char> operators;
    int n = s.length();
    int currentNumber = 0;
    
    for (int i = 0; i < n; i++) {
        char c = s[i];
        
        // If the current character is a digit, form the whole number
        if (isdigit(c)) {
            currentNumber = currentNumber * 10 + (c - '0');
        }
        
        // If the current character is an operator, or it's the last character
        if (!isdigit(c) && !isspace(c) || i == n - 1) {
            if (!operators.empty()) {
                // If the previous operator was '*' or '/', perform that operation
                char prevOp = operators.top();
                if (prevOp == '*' || prevOp == '/') {
                    operators.pop();
                    int prevNumber = numbers.top();
                    numbers.pop();
                    currentNumber = (prevOp == '*') ? prevNumber * currentNumber : prevNumber / currentNumber;
                }
            }
            numbers.push(currentNumber);
            currentNumber = 0;
            if (i < n - 1) {
                operators.push(c);
            }
        }
    }
    
    // Now process remaining operators for addition or subtraction
    int result = numbers.top();
    numbers.pop();
    while (!numbers.empty()) {
        char op = operators.top();
        operators.pop();
        int num = numbers.top();
        numbers.pop();
        if (op == '+') {
            result += num;
        } else if (op == '-') {
            result = num - result;
        }
    }
    
    return result;
}

int main() {
    std::string expression = "3+2*2";
    std::cout << calculate(expression) << std::endl; // Output: 7
    return 0;
}
```

#### Complexity:
- **Time Complexity:** O(n), where n is the length of the string, since we are iterating once through the input.
- **Space Complexity:** O(n), due to the supplementary space used by two stacks.

---

### Approach 2: Optimized Approach: Using a single stack

#### Intuition:
This approach leverages a single stack to avoid excessive space usage and simplify the handling of operator precedence directly during traversal of the string. By peeking at the previous operator, we can decide to immediately calculate the multiplication and division results when encountered and store intermediate sums and subtotals as needed.

#### Steps:
1. Initialize a single stack to keep track of numbers.
2. Keep track of the current number being processed and the last operator encountered.
3. Traverse through the string character by character.
4. If a digit, continue forming the number.
5. If an operator or end of string, perform depending on the operator:
   - If '+' or '-', push the number with the appropriate sign into the stack.
   - If '*' or '/', compute the result with the top of the stack and push it back.
6. Sum up all values from the stack as the final result.

#### Code:

```cpp
#include <iostream>
#include <stack>
#include <string>

int calculate(std::string s) {
    std::stack<int> stack;
    int currentNumber = 0;
    char operation = '+';
    int n = s.size();

    for (int i = 0; i < n; i++) {
        char c = s[i];

        if (isdigit(c)) {
            currentNumber = currentNumber * 10 + (c - '0');
        }

        if (!isdigit(c) && !isspace(c) || i == n - 1) {
            if (operation == '-') {
                stack.push(-currentNumber);
            } else if (operation == '+') {
                stack.push(currentNumber);
            } else if (operation == '*') {
                int top = stack.top();
                stack.pop();
                stack.push(top * currentNumber);
            } else if (operation == '/') {
                int top = stack.top();
                stack.pop();
                stack.push(top / currentNumber);
            }
            operation = c;
            currentNumber = 0;
        }
    }

    int result = 0;
    while (!stack.empty()) {
        result += stack.top();
        stack.pop();
    }

    return result;
}

int main() {
    std::string expression = "3+2*2";
    std::cout << calculate(expression) << std::endl; // Output: 7
    return 0;
}
```

#### Complexity:
- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(n), where n is the number of elements pushed onto the stack. However, this can be improved effectively with logic modifications depending on use-case scenarios. 

By using a single stack, we ensure an efficient traversal and manageable space usage all while maintaining clarity and simplicity in evaluating expressions with only basic arithmetic operations and considering operator precedence appropriately.

