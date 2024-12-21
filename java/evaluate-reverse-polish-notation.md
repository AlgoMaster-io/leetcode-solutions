# [Evaluate Reverse Polish Notation - LeetCode](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

## Approaches
- [Approach 1: Stack-based Evaluation](#approach-1-stack-based-evaluation)
- [Approach 2: Two Stacks for Operators and Operands](#approach-2-two-stacks-for-operators-and-operands) *(Not recommended but educational)*

---

### Approach 1: Stack-based Evaluation

**Intuition:**

The Reverse Polish Notation (RPN) is a mathematical notation in which every operator follows all of its operands. This makes it very straightforward to evaluate using a stack, as stacks naturally follow the Last-In-First-Out (LIFO) principle.

- Traverse the tokens:
  - If the token is a number, push it onto the stack.
  - If the token is an operator, pop the necessary operands from the stack, perform the operation, and push the result back onto the stack.

By the end of the process, the stack should contain just one element - the result of the expression.

**Detailed Steps:**

1. Initialize an empty stack to hold integers.
2. Iterate over each token:
   - If it's a number, convert it to an integer and push onto the stack.
   - If it's an operator (`+`, `-`, `*`, `/`), pop the top two operands from the stack, apply the operator, and push the result back onto the stack.
3. At the end of iteration, the stack will contain one element which is the result.

**Java Code:**

```java
import java.util.Stack;

public class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();

        for (String token : tokens) {
            if (isOperator(token)) {
                int b = stack.pop();
                int a = stack.pop();
                int result = 0;
                
                switch (token) {
                    case "+":
                        result = a + b;
                        break;
                    case "-":
                        result = a - b;
                        break;
                    case "*":
                        result = a * b;
                        break;
                    case "/":
                        result = a / b; // assume b is not zero
                        break;
                }
                
                stack.push(result);
            } else {
                // convert token to integer and push to stack
                stack.push(Integer.valueOf(token));
            }
        }
        
        // final result is the last item in the stack
        return stack.pop();
    }

    private boolean isOperator(String token) {
        return token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/");
    }
}
```
**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the number of tokens. Each token is processed once.
- **Space Complexity:** O(n), in the worst case, if all tokens are numbers, the stack size could be n.

### Approach 2: Two Stacks for Operators and Operands (Not recommended but educational)

**Intuition:**

This approach is more verbose and less optimal but serves as an educational alternative showing a crude separation of operators from operands, similar to parsing a regular infix expression.

- One stack for operands and one for operators would be used. However, this makes the algorithm unnecessarily convoluted for RPN.

**Detailed Steps and Analysis:**

Upon investigating the way reverse polish notations work, it becomes evident that using two stacks adds overhead without simplifying the logic contrary to how traditional expression parsing might benefit from such separation.

This method is purely conceptual for learning purposes only and is not implemented here due to its inefficiency compared to the first approach.

**Conclusion:**

- While the two stack method might intrigue those interested in how expression evaluators might be implemented in infix notation parsing, it is impractical for RPN and shows the elegance of the Stack-based Evaluation approach. 

The first approach remains the most straightforward and optimal for the nature of the problem.

