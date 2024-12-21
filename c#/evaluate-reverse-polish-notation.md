# [Leetcode 150: Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches
- [Approach 1: Stack-based Evaluation](#approach-1-stack-based-evaluation)

## Approach 1: Stack-based Evaluation

### Intuition
Reverse Polish Notation (RPN) is a mathematical notation in which operators follow their operands. For example, the expression `2 3 +` would result in `5`. The idea is to use a stack to evaluate the expression from left to right:

1. Traverse through each token in the expression.
2. If the current token is a number, push it onto the stack.
3. If the current token is an operator, pop operands from the stack, apply the operator, and push the result back onto the stack.
4. The final result will be the only element remaining in the stack.

### Code
```csharp
public class Solution {
    public int EvalRPN(string[] tokens) {
        Stack<int> stack = new Stack<int>();

        foreach (string token in tokens) {
            // Check if the current token is an operator
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                // Pop two operands from the stack
                int b = stack.Pop();
                int a = stack.Pop();
                
                // Perform the operation and push the result back to the stack
                switch (token) {
                    case "+":
                        stack.Push(a + b);
                        break;
                    case "-":
                        stack.Push(a - b);
                        break;
                    case "*":
                        stack.Push(a * b);
                        break;
                    case "/":
                        stack.Push(a / b); // Note: Division in C# is integer division
                        break;
                }
            } else {
                // It's a number, push it to the stack
                stack.Push(int.Parse(token));
            }
        }

        // The result is the only value left in the stack
        return stack.Pop();
    }
}
```

### Important Steps:
- **Parsing numbers and operators:** `int.Parse(token)` is used to convert string numbers to integers.
- **Using a stack:** The stack is used to manage the operands and intermediate results.
- **Operator handling:** Ensure two operands are popped and handled properly for each operator.
- **Final result:** At the end of the process, the stack contains a single value: the result.

### Time Complexity
The time complexity of this approach is O(n), where n is the number of tokens in the RPN expression, because each token is processed once.

### Space Complexity
The space complexity is O(n) as well, due to the worst case scenario where all tokens (numbers) are pushed onto the stack.

