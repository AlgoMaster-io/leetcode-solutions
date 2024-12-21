# [Leetcode 150: Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches:
- [Approach 1: Stack & Iteration](#approach-1-stack-iteration)

---

## Approach 1: Stack & Iteration

### Intuition
The Reverse Polish Notation (RPN) is a mathematical notation where every operator follows all of its operands. This postfix expression can be evaluated easily using a stack because operands are always pushed, and when an operator is encountered, the necessary number of operands can be popped from the stack for evaluation.

1. We iterate through each token in the input array.
2. If it's a number, push it onto the stack.
3. If it's an operator, pop the necessary operands (for basic arithmetic operations, always two operands), perform the operation, and push the result back onto the stack.
4. The final entry in the stack will be the result of the expression.

### Code

```go
func evalRPN(tokens []string) int {
    stack := []int{}

    // Helper function to check if a token is an operator
    isOperator := func(op string) bool {
        return op == "+" || op == "-" || op == "*" || op == "/"
    }

    // Iterate over each token
    for _, token := range tokens {
        if !isOperator(token) {
            // Convert the number and push onto stack
            num, _ := strconv.Atoi(token)
            stack = append(stack, num)
        } else {
            // Pop the top two elements from stack for operation
            right := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            left := stack[len(stack)-1]
            stack = stack[:len(stack)-1]

            // Perform the operation based on the current operator
            result := 0
            if token == "+" {
                result = left + right
            } else if token == "-" {
                result = left - right
            } else if token == "*" {
                result = left * right
            } else if token == "/" {
                result = left / right
            }

            // Push the result back onto the stack
            stack = append(stack, result)
        }
    }

    // Resultant single number left on the stack is the answer
    return stack[0]
}
```

### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of tokens in the input list. We process each token exactly once.
- **Space Complexity:** O(n), in the worst case, if all tokens are numbers, they are stored in the stack.

