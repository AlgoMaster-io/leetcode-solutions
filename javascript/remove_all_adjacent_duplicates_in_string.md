# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var removeDuplicates = function(s) {
    const stack = [];

    // Iterate through the string
    for (let c of s) {
        // If the stack is not empty and the top element matches the current character
        if (stack.length > 0 && stack[stack.length - 1] === c) {
            stack.pop(); // Remove the top element
        } else {
            stack.push(c); // Add the current character to the stack
        }
    }

    return stack.join(''); // Convert stack back to a string
};
```

