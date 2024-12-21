# [Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
- [Approach 1: Using a Stack](#approach-1-using-a-stack)
- [Approach 2: In-Place Simulation](#approach-2-in-place-simulation)

## Approach 1: Using a Stack

**Intuition**:  

The idea is to use a stack to help remove duplicates. While iterating through the string, push the current character onto the stack if it is different from the top of the stack (i.e., does not match the last character added to the result). If it does match, it means we have found a duplicate pair, so we pop the stack instead.

**JavaScript Code**:

```javascript
function removeDuplicates(s) {
    // Initialize an empty stack to keep track of characters
    const stack = [];
    
    // Iterate through each character in the string
    for (const char of s) {
        // If the stack is not empty and the top of the stack equals the current character
        // it means we have found a duplicate pair, so we pop the stack
        if (stack.length && stack[stack.length - 1] === char) {
            stack.pop();
        } else {
            // Otherwise, we push the current character onto the stack
            stack.push(char);
        }
    }
    
    // Convert the stack back to a string and return
    return stack.join('');
}
```

- **Time Complexity**: O(n), where n is the length of the string, because each character is pushed and popped from the stack at most once.
- **Space Complexity**: O(n) in the worst case, if no duplicates exist and all characters are stored in the stack.

## Approach 2: In-Place Simulation

**Intuition**:  

This approach simulates the stack solution but modifies the string directly using a result array to simulate stack behavior. We iterate through the string and keep track of the index where we would "push" characters onto the stack. If the current character matches the last character in the result, we decrease the index (simulating a stack "pop"). Otherwise, we overwrite the character at the current index and increment it.

**JavaScript Code**:

```javascript
function removeDuplicates(s) {
    // Convert string to array to simulate in-place operations
    const result = Array.from(s);
    let i = 0;
    
    // Iterate through each character in the string
    for (const char of s) {
        // If index i is bigger than 0 and current character matches previous character
        // it means we've found a duplicate pair, so we reduce the index i
        if (i > 0 && result[i - 1] === char) {
            i--; 
        } else {
            // Otherwise, put the current character at index i and increase the index
            result[i] = char;
            i++;
        }
    }
    
    // Use the current valid filled portion of result to construct final answer
    return result.slice(0, i).join('');
}
```

- **Time Complexity**: O(n), where n is the length of the string. We go through the string exactly once.
- **Space Complexity**: O(n) because we use an array to hold the results. However, this can be seen as O(1) extra space since we're using a direct modification of the input string (in this context of transforming a mutable version).

This covers both approaches for solving the problem using stacks and in-place simulation. Both effectively utilize stack-like logic to identify and remove adjacent duplicates efficiently.

