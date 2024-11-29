# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution

typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    public isValid(s: string): boolean {
        const stack: string[] = [];

        // Iterate through the string
        for (let i = 0; i < s.length; i++) {
            const c = s[i];
            // Push opening brackets onto the stack
            if (c === '(' || c === '{' || c === '[') {
                stack.push(c);
            }
            // Check closing brackets
            else {
                // If stack is empty or doesn't match the top, return false
                if (stack.length === 0 || !this.isMatchingPair(stack.pop()!, c)) {
                    return false;
                }
            }
        }

        // If the stack is empty, all brackets are matched
        return stack.length === 0;
    }

    private isMatchingPair(open: string, close: string): boolean {
        return (open === '(' && close === ')') ||
               (open === '{' && close === '}') ||
               (open === '[' && close === ']');
    }
}
```

