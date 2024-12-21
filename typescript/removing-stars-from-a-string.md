# [Leetcode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Navigation

- [Approach 1: Stack Simulation](#approach-1-stack-simulation)
- [Approach 2: Two Pointer Simulation](#approach-2-two-pointer-simulation)

---

## Approach 1: Stack Simulation

### Intuition

This problem is reminiscent of processing a sequence of undo operations, where '*' acts as a delete operation. The most straightforward way to simulate this process is by using a stack. Each non-star character is added to the stack, and whenever a '*' is encountered, the top of the stack is removed. This way, the stack at the end will contain our desired string without stars and their preceding characters.

### Implementation

```typescript
function removeStars(s: string): string {
    // Create an empty array to use as a stack.
    const stack: string[] = [];
    
    // Iterate through each character in the input string.
    for (const char of s) {
        // If the character is '*', pop the last element from the stack.
        // This simulates the "removing" of a character.
        if (char === '*') {
            stack.pop();
        } else {
            // If it's a regular character, push it onto the stack.
            stack.push(char);
        }
    }
    
    // Join all characters in the stack to form the resulting string.
    return stack.join('');
}
```

### Complexity Analysis

- **Time Complexity:** O(n), where n is the length of the string. We process each character of the string exactly once.
- **Space Complexity:** O(n), since in the worst case (no stars), we store all characters in the stack.

---

## Approach 2: Two Pointer Simulation

### Intuition

We can simulate the stack with a two-pointer approach. Essentially, we will keep a pointer `writeIdx` that keeps track of where the next character should be written in the result. As we process each character, if it is not a star, we place it at the `writeIdx` and increment the `writeIdx`. If it is a star, we decrement the `writeIdx`, effectively "removing" the last valid character written.

### Implementation

```typescript
function removeStarsTwoPointer(s: string): string {
    // Create an array to hold the final string characters.
    const result: string[] = new Array(s.length);
    let writeIdx = 0; // This is our pointer that writes the characters back.
    
    // Iterate over each character in the string.
    for (const char of s) {
        if (char === '*') {
            // Decrement writeIdx to remove the previously added character.
            writeIdx--;
        } else {
            // Write the character at the current writeIdx position.
            result[writeIdx] = char;
            // Increment writeIdx to prepare for the next character.
            writeIdx++;
        }
    }
    
    // The result array up to writeIdx is our final string.
    return result.slice(0, writeIdx).join('');
}
```

### Complexity Analysis

- **Time Complexity:** O(n), as we only pass through the string once.
- **Space Complexity:** O(n), because we store the resulting characters in an array with similar length.

Both approaches yield the correct solution; however, the two-pointer approach might be slightly more space-efficient in practical implementations due to reduced overhead compared to using array methods in the stack approach.

