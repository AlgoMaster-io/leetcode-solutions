# [Leetcode 150: Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches:

1. [Stack-Based Evaluation](#stack-based-evaluation)

---

### Stack-Based Evaluation

#### Intuition:
The Reverse Polish Notation (RPN) is a mathematical notation in which every operator follows all of its operands. This property naturally suggests using a stack data structure. Stacks are LIFO (Last In, First Out), making them a perfect fit for evaluating expressions where operators work on the last two numbers encountered.

Here's the step-by-step approach:

1. Traverse each token in the list of tokens.
2. If a token is an operand (number), push it onto the stack.
3. If a token is an operator, pop the top two operands from the stack, apply the operator, and push the result back onto the stack.
4. After processing all tokens, the stack will contain one element—the result of the expression.

#### Detailed Code with Comments:

```python
def evalRPN(tokens):
    # This will be our stack to store operands
    stack = []
    
    # Function to perform arithmetic operations
    def operate(op1, op2, operator):
        if operator == '+':
            return op1 + op2
        elif operator == '-':
            return op1 - op2
        elif operator == '*':
            return op1 * op2
        elif operator == '/':
            # Python division operator keeps float, use int() for truncation towards zero
            return int(op1 / op2)
    
    # Iterate over each token
    for token in tokens:
        if token in "+-*/":
            # Pop the last two operands from the stack for the operation
            op2 = stack.pop()
            op1 = stack.pop()
            # Perform operation and push result back to stack
            result = operate(op1, op2, token)
            stack.append(result)
        else:
            # It's a number, convert it, and push it onto the stack
            stack.append(int(token))
    
    # The last remaining item in the stack is the result
    return stack[0]
```

#### Complexity Analysis:

- **Time Complexity:** O(n), where n is the number of tokens. Each token is processed once.
- **Space Complexity:** O(n), in the worst case, we might store all tokens in the stack if all are numbers followed by operators.

This approach efficiently evaluates the RPN expression using a stack, handling integer division correctly using Python's `int()` for truncation.

