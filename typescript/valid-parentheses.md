[Leetcode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approaches:
- [Approach 1: Stack-based Solution](#approach-1-stack-based-solution)

### Approach 1: Stack-based Solution

**Intuition:**

The problem is essentially about matching pairs. Given the nature of parentheses, one common approach is to use a stack data structure. A stack is last in first out (LIFO), which is ideal for cases like this where you need to match the most recent opening parenthesis with a closing one.

**Approach:**

- Iterate through each character of the string.
- Whenever an opening bracket is encountered (`(`, `{`, or `[`), push it onto the stack.
- Whenever a closing bracket is encountered, check if the top of the stack is the matching opening bracket.
  - If it matches, pop the top element from the stack.
  - If it doesn't match or the stack is empty, the string is not valid.
- After processing all characters, check if the stack is empty. If it is, the string is valid, otherwise, it is not.

**Algorithm:**

1. Create a stack to hold the opening brackets.
2. Create a map or object to define matching pairs.
3. Iterate through characters in the string:
   - If the character is an opening bracket, push it onto the stack.
   - If it's a closing bracket, check the top element of the stack for a match.
4. Finally, check if the stack is empty to determine if the string is valid.

**Time Complexity:** O(n) where n is the length of the string, as we traverse the string once.

**Space Complexity:** O(n) in the worst case when all characters are opening brackets.

```typescript
function isValid(s: string): boolean {
    // Map to store matching pairs
    const matchingBrackets: { [key: string]: string } = {
        '(': ')',
        '{': '}',
        '[': ']'
    };

    // Stack to store opening brackets
    const stack: string[] = [];

    // Iterate over each character in the string
    for (let char of s) {
        // If it's an opening bracket
        if (matchingBrackets[char]) {
            // Push onto stack
            stack.push(char);
        } else {
            // If it's a closing bracket
            // Check if there's a matching opening bracket at the top of the stack
            const topElement = stack.length > 0 ? stack.pop() : '#'; // Use '#' to handle underflow
            if (char !== matchingBrackets[topElement!]) {
                // If it doesn't match, return false
                return false;
            }
        }
    }
    
    // If the stack is empty, all brackets were matched correctly
    return stack.length === 0;
}
```

This solution leverages a stack to ensure that every opening bracket has a corresponding and correctly ordered closing bracket, efficiently validating the input string.

