# [Leetcode 150: Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches
- [Approach 1: Stack-based Solution](#approach-1-stack-based-solution)

---

## Approach 1: Stack-based Solution

### Intuition
Reverse Polish Notation (RPN) is a mathematical notation in which every operator follows all of its operands. This notation has no need for parenthesis as long as operator arity is fixed. Hence, this problem is best approached using a stack data structure since you can easily manipulate the last operand(s) when an operator is encountered.

### Algorithm
- Use a stack to store operands until an operator is encountered.
- Iterate through each token in the input list.
- If the token is a number, push it to the stack.
- If the token is an operator, pop the required number of operands from the stack, perform the operation, and push the result back to the stack.
- At the end, the stack will contain exactly one element, which is the result of the expression.

### Code
```javascript
var evalRPN = function(tokens) {
    // Initialize the stack to store operands
    let stack = [];
    
    // Iterate through each token in the given RPN expression
    for (let token of tokens) {
        // Check if the current token is an operator
        if (token === '+' || token === '-' || token === '*' || token === '/') {
            // Pop the last two operands from the stack
            const b = stack.pop();
            const a = stack.pop();
            
            // Evaluate the expression based on the operator
            switch (token) {
                case '+':
                    stack.push(a + b);
                    break;
                case '-':
                    stack.push(a - b);
                    break;
                case '*':
                    stack.push(a * b);
                    break;
                case '/':
                    // Perform integer division
                    stack.push(Math.trunc(a / b));
                    break;
            }
        } else {
            // If it's a number, parse and push it onto the stack
            stack.push(parseInt(token));
        }
    }
    
    // The result of the expression will be the only element left in the stack
    return stack.pop();
};
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of tokens. Each token is processed once and operations on the stack (push and pop) take O(1) time.
- **Space Complexity:** O(n) in the worst case, which occurs when the expression has many numbers followed by a single operator at the end. The stack will store all those numbers before one operation reduces them to a single result.

