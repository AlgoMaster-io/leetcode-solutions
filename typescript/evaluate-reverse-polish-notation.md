# [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Solutions
- [Approach 1: Stack-based Evaluation (Basic Solution)](#approach-1-stack-based-evaluation-basic-solution)

### Approach 1: Stack-based Evaluation (Basic Solution)

**Intuition:**

Reverse Polish Notation (RPN) is a mathematical notation in which every operator follows all of its operands. For this approach, we'll use a stack to evaluate expressions as follows:
1. Traverse through the tokens.
2. If a token is an operand (number), push it onto the stack.
3. If a token is an operator, pop the top two operands from the stack, evaluate the expression and push the result back onto the stack.
4. At the end, the stack will contain one element which is the result.

In this approach, the stack data structure seamlessly maintains the precedence of operators and operands.

**Steps:**
1. Initialize an empty stack. 
2. Iterate over each token in the input:
   - If the token is a number, push it to the stack.
   - If the token is an operator, pop the required operands from the stack, apply the operator, and push the result back onto the stack.
3. Finally, pop the result from the stack, which is the evaluated output.

**TypeScript Implementation:**

```typescript
function evalRPN(tokens: string[]): number {
    // Initialize a stack to store numbers
    const stack: number[] = [];
    // Iterate over each token
    tokens.forEach(token => {
        if (!isNaN(Number(token))) {
            // If the token is a number, push it to the stack
            stack.push(Number(token));
        } else {
            // The token is an operator, pop two top numbers from the stack
            const b = stack.pop()!;
            const a = stack.pop()!;
            let result: number;
            
            // Evaluate the results based on the operator
            if (token === '+') {
                result = a + b;
            } else if (token === '-') {
                result = a - b;
            } else if (token === '*') {
                result = a * b;
            } else if (token === '/') {
                // Integer division, truncate towards zero
                result = a / b;
                result = result > 0 ? Math.floor(result) : Math.ceil(result);
            }
            // Push the computed result back onto the stack
            stack.push(result);
        }
    });
    // The last element in the stack is the result
    return stack.pop()!;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of tokens. We process each token at most once.
- **Space Complexity:** O(n), in the worst case, all elements except the operators will be pushed onto the stack. 

This solution efficiently evaluates expressions in RPN using a stack, handling each operation in constant time relative to the number of tokens.

