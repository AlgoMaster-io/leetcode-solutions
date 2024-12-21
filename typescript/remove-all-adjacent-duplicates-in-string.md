# [Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
- [Approach 1: Using a Stack](#approach-1-using-a-stack)

---

## Approach 1: Using a Stack

### Intuition
The problem requires us to remove adjacent duplicates iteratively until no more can be removed. One common and effective approach is to use a stack. The stack data structure will help efficiently track the sequence of characters and remove duplicates as we process the string.

### Detailed Explanation
- Initialize an empty array called `stack` to simulate stack operations.
- Iterate through each character in the input string:
  - If the stack is not empty and the top of the stack is equal to the current character, it means we found a duplicate. Thus, we remove (pop) the top of the stack.
  - If the stack is empty or the top of the stack is not equal to the current character, we push the current character onto the stack.
- By the end of the iteration, the stack will contain only those characters that are not adjacent duplicates. Therefore, join the stack elements to form the resulting string.

### Code
```typescript
function removeDuplicates(s: string): string {
    // Stack to store characters with no adjacent duplicates
    const stack: string[] = [];
    
    // Iterate through each character in the input string
    for (let char of s) {
        // Check if the current character is the same as the top of the stack
        if (stack.length > 0 && stack[stack.length - 1] === char) {
            // If yes, pop the top of the stack (remove duplicate)
            stack.pop();
        } else {
            // If no, push the current character onto the stack
            stack.push(char);
        }
    }
    
    // Join the stack to form the result string
    return stack.join('');
}
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the length of the string. Each character is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), as in the worst case, all characters are stored in the stack if no duplicates are present.

This approach efficiently removes adjacent duplicates while only making a single pass through the string, making it both time-efficient and easy to understand using the stack data structure.

