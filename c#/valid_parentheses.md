# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public bool IsValid(string s) {
        Stack<char> stack = new Stack<char>();

        // Iterate through the string
        foreach (char c in s) {
            // Push opening brackets onto the stack
            if (c == '(' || c == '{' || c == '[') {
                stack.Push(c);
            }
            // Check closing brackets
            else {
                // If stack is empty or doesn't match the top, return false
                if (stack.Count == 0 || !IsMatchingPair(stack.Pop(), c)) {
                    return false;
                }
            }
        }

        // If the stack is empty, all brackets are matched
        return stack.Count == 0;
    }

    private bool IsMatchingPair(char open, char close) {
        return (open == '(' && close == ')') ||
               (open == '{' && close == '}') ||
               (open == '[' && close == ']');
    }
}
```

