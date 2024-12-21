[Leetcode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### Approaches:

- [Approach 1: Stack-based Approach](#approach-1-stack-based-approach)

---

## Approach 1: Stack-based Approach

### Intuition

When faced with parentheses, a frequent requirement is to determine if they are "balanced" or "properly nested." A stack is a natural data structure to use when checking for balanced parentheses because it provides a Last-In-First-Out (LIFO) order, which matches the nested nature of parentheses.

The idea is to traverse the input string, pushing opening brackets onto a stack and popping when closing brackets are encountered. For each closing bracket, we check if it matches the top of the stack, which should be its corresponding opening bracket. If the stack is empty by the end of the traversal, the string was properly nested; otherwise, it was not.

### Algorithm

1. Initialize an empty stack.
2. Traverse each character in the input string:
   - If it's an opening bracket (`(`, `{`, `[`), push it onto the stack.
   - If it's a closing bracket (`)`, `}`, `]`):
     - Check if the stack is empty. If it is, return `false`.
     - Pop the top item from the stack.
     - Ensure that the popped element is the matching opening bracket. If not, return `false`.
3. After processing all characters, check if the stack is empty. If it is, all brackets were matched appropriately, so return `true`. Otherwise, return `false`.

```javascript
function isValid(s) {
    // Initializing a stack to keep track of the open parentheses
    const stack = [];
    // A map to define the mapping of each closing bracket to the corresponding opening bracket
    const map = {
        ')': '(',
        '}': '{',
        ']': '['
    };

    // Iterate through each character in the string
    for (let char of s) {
        // If the character is one of the closing brackets.
        if (char in map) {
            // Pop the top element from the stack if it is not empty, else assign a dummy value '#'
            const topElement = stack.length > 0 ? stack.pop() : '#';
            // Check the matching, if mismatched return false
            if (topElement !== map[char]) {
                return false;
            }
        } else {
            // If it's an opening bracket, push it onto the stack
            stack.push(char);
        }
    }

    // If the stack is empty, all the opened brackets are properly closed
    return stack.length === 0;
}
```

### Time Complexity

- The algorithm makes a single linear scan of the string. Both the time complexity for traversing the string and the average time complexity for push/pop operations on a stack are O(1). Hence, the overall time complexity is O(n).

### Space Complexity

- We only use a stack to store opening brackets, which is O(n) in the worst case when all the characters are opening brackets.

By using a stack, we leverage a fundamental property of balanced symbols and efficiently solve the problem with a straightforward algorithm. This approach is both optimal and effective for this type of problem.

