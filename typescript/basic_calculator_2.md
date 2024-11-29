# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution

typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

class Solution {
    calculate(s: string): number {
        const stack: number[] = [];
        let currentNumber = 0;
        let operation = '+'; // Default operation is addition

        for (let i = 0; i < s.length; i++) {
            const c = s.charAt(i);

            // Build the current number
            if (!isNaN(Number(c)) && c !== ' ') {
                currentNumber = currentNumber * 10 + (c.charCodeAt(0) - '0'.charCodeAt(0));
            }

            // Process operators and the last number
            if (isNaN(Number(c)) && c !== ' ' || i === s.length - 1) {
                if (operation === '+') {
                    stack.push(currentNumber);
                } else if (operation === '-') {
                    stack.push(-currentNumber);
                } else if (operation === '*') {
                    stack.push(stack.pop()! * currentNumber);
                } else if (operation === '/') {
                    stack.push(Math.trunc(stack.pop()! / currentNumber));
                }

                operation = c; // Update the operation
                currentNumber = 0; // Reset the current number
            }
        }

        // Sum up all the values in the stack
        let result = 0;
        while (stack.length !== 0) {
            result += stack.pop()!;
        }

        return result;
    }
}
```

