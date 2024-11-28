# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Stack;

public class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();

        // Iterate through the string
        for (char c : s.toCharArray()) {
            // Push opening brackets onto the stack
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            }
            // Check closing brackets
            else {
                // If stack is empty or doesn't match the top, return false
                if (stack.isEmpty() || !isMatchingPair(stack.pop(), c)) {
                    return false;
                }
            }
        }

        // If the stack is empty, all brackets are matched
        return stack.isEmpty();
    }

    private boolean isMatchingPair(char open, char close) {
        return (open == '(' && close == ')') ||
               (open == '{' && close == '}') ||
               (open == '[' && close == ']');
    }
}
```