# 1047. [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    removeDuplicates(s: string): string {
        const stack: string[] = [];

        // Iterate through the string
        for (const c of s) {
            // If the stack is not empty and the top element matches the current character
            if (stack.length > 0 && stack[stack.length - 1] === c) {
                stack.pop(); // Remove the top element
            } else {
                stack.push(c); // Add the current character to the stack
            }
        }

        return stack.join(''); // Convert stack back to a string
    }
}
```

