# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var calculate = function(s) {
    let stack = [];
    let currentNumber = 0;
    let operation = '+'; // Default operation is addition

    for (let i = 0; i < s.length; i++) {
        let c = s[i];

        // Build the current number
        if (!isNaN(c) && c !== ' ') {
            currentNumber = currentNumber * 10 + parseInt(c);
        }

        // Process operators and the last number
        if (isNaN(c) && c !== ' ' || i === s.length - 1) {
            if (operation === '+') {
                stack.push(currentNumber);
            } else if (operation === '-') {
                stack.push(-currentNumber);
            } else if (operation === '*') {
                stack.push(stack.pop() * currentNumber);
            } else if (operation === '/') {
                stack.push(Math.trunc(stack.pop() / currentNumber));
            }

            operation = c; // Update the operation
            currentNumber = 0; // Reset the current number
        }
    }

    // Sum up all the values in the stack
    let result = 0;
    while (stack.length > 0) {
        result += stack.pop();
    }

    return result;
};
```

