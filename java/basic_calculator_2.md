# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Stack;

public class Solution {
    public int calculate(String s) {
        Stack<Integer> stack = new Stack<>();
        int currentNumber = 0;
        char operation = '+'; // Default operation is addition

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // Build the current number
            if (Character.isDigit(c)) {
                currentNumber = currentNumber * 10 + (c - '0');
            }

            // Process operators and the last number
            if (!Character.isDigit(c) && c != ' ' || i == s.length() - 1) {
                if (operation == '+') {
                    stack.push(currentNumber);
                } else if (operation == '-') {
                    stack.push(-currentNumber);
                } else if (operation == '*') {
                    stack.push(stack.pop() * currentNumber);
                } else if (operation == '/') {
                    stack.push(stack.pop() / currentNumber);
                }

                operation = c; // Update the operation
                currentNumber = 0; // Reset the current number
            }
        }

        // Sum up all the values in the stack
        int result = 0;
        while (!stack.isEmpty()) {
            result += stack.pop();
        }

        return result;
    }
}
```