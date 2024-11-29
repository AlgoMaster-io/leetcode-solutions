# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1) (fixed-size stack and frequency array)

function removeDuplicateLetters(s: string): string {
    const lastOccurrence: number[] = new Array(26).fill(0); // Store last index of each character
    const inStack: boolean[] = new Array(26).fill(false); // Track if character is already in the stack

    // Calculate last occurrence of each character
    for (let i = 0; i < s.length; i++) {
        lastOccurrence[s.charCodeAt(i) - 'a'.charCodeAt(0)] = i;
    }

    const stack: string[] = [];

    // Iterate through the string
    for (let i = 0; i < s.length; i++) {
        const c = s[i];

        // Skip if the character is already in the stack
        if (inStack[c.charCodeAt(0) - 'a'.charCodeAt(0)]) {
            continue;
        }

        // Remove characters from the stack that are greater than the current character
        // and can appear later in the string
        while (
            stack.length > 0 &&
            stack[stack.length - 1] > c &&
            lastOccurrence[stack[stack.length - 1].charCodeAt(0) - 'a'.charCodeAt(0)] > i
        ) {
            inStack[stack.pop()!.charCodeAt(0) - 'a'.charCodeAt(0)] = false;
        }

        // Push the current character onto the stack
        stack.push(c);
        inStack[c.charCodeAt(0) - 'a'.charCodeAt(0)] = true;
    }

    // Build the result string from the stack
    return stack.join('');
}
```

