# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int Calculate(string s) {
        Stack<int> stack = new Stack<int>();
        int currentNumber = 0;
        char operation = '+'; // Default operation is addition

        for (int i = 0; i < s.Length; i++) {
            char c = s[i];

            // Build the current number
            if (char.IsDigit(c)) {
                currentNumber = currentNumber * 10 + (c - '0');
            }

            // Process operators and the last number
            if (!char.IsDigit(c) && c != ' ' || i == s.Length - 1) {
                if (operation == '+') {
                    stack.Push(currentNumber);
                } else if (operation == '-') {
                    stack.Push(-currentNumber);
                } else if (operation == '*') {
                    stack.Push(stack.Pop() * currentNumber);
                } else if (operation == '/') {
                    stack.Push(stack.Pop() / currentNumber);
                }

                operation = c; // Update the operation
                currentNumber = 0; // Reset the current number
            }
        }

        // Sum up all the values in the stack
        int result = 0;
        while (stack.Count > 0) {
            result += stack.Pop();
        }

        return result;
    }
}
```

