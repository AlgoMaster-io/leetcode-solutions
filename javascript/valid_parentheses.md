# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    isValid(s) {
        const stack = [];

        // Helper function to check for matching pairs
        const isMatchingPair = (open, close) => {
            return (open === '(' && close === ')') ||
                   (open === '{' && close === '}') ||
                   (open === '[' && close === ']');
        };

        // Iterate through the string
        for (let c of s) {
            // Push opening brackets onto the stack
            if (c === '(' || c === '{' || c === '[') {
                stack.push(c);
            }
            // Check closing brackets
            else {
                // If stack is empty or doesn't match the top, return false
                if (stack.length === 0 || !isMatchingPair(stack.pop(), c)) {
                    return false;
                }
            }
        }

        // If the stack is empty, all brackets are matched
        return stack.length === 0;
    }
}
```

