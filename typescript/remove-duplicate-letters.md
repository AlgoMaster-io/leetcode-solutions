# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

### Approaches:
1. [Greedy Approach with Stack](#greedy-approach-with-stack)
2. [Improved Greedy Approach with Frequency Count](#improved-greedy-approach-with-frequency-count)

## Greedy Approach with Stack

### Intuition:
The problem requires us to remove duplicate letters and return the smallest lexicographical result. We use a greedy approach here by employing a stack to build the result. The idea is to maintain the order while ensuring we use each character only once and that it's the first smallest lexicographical order possible.

### Steps:
1. **Create a Stack:** Use a stack to maintain the sequence of characters for the result.
2. **Inclusion Set:** Maintain a set to check if a character is already in the stack.
3. **Iterate over the String:** For each character:
   - If it's already in the stack, continue to the next character.
   - While the stack is not empty, the top element is greater than the current character, and the top element can still appear later (more counts left), pop the stack.
   - Add the current character to the stack and mark it as included.
4. **Construct Result:** Convert the stack to a string to get the final answer.

### Code:
```typescript
function removeDuplicateLetters(s: string): string {
    const stack: string[] = [];
    const included = new Set<string>();
    const lastOccurrence = new Map<string, number>();
    
    // Record the last occurrence of each character.
    for (let i = 0; i < s.length; i++) {
        lastOccurrence.set(s[i], i);
    }
    
    // Iterate through each character in the string.
    for (let i = 0; i < s.length; i++) {
        const currentChar = s[i];
        
        // If current character is already in stack, skip it.
        if (included.has(currentChar)) continue;
        
        // Greedily ensure stack is in lexicographical order.
        while (stack.length > 0 && stack[stack.length - 1] > currentChar && lastOccurrence.get(stack[stack.length - 1])! > i) {
            const removedChar = stack.pop()!;
            included.delete(removedChar);
        }
        
        // Add current character to stack and mark it as included.
        stack.push(currentChar);
        included.add(currentChar);
    }
    
    return stack.join('');
}
```

### Time Complexity:
- **O(n):** We process each character at most twice (once pushing to the stack and once popping).
- **Space Complexity:** O(1), only 26 lowercase letters in the stack and set.

---
## Improved Greedy Approach with Frequency Count

### Intuition:
We can optimize the previous approach slightly by keeping track of the frequency of each character directly, instead of recalculating occurrences.

### Steps:
1. **Frequency Map:** Calculate the frequency of each character.
2. **Inclusion Set & Stack:** Similar to the previous solution, manage a stack and a set to track included characters.
3. **Iterate over the String:** For each character:
   - Decrement its count in the frequency map.
   - If it's already in the stack, skip to the next character.
   - Pop from the stack while the current character is better placed lexicographically and previous character is still needed.
   - Push the current character onto the stack.

### Code:
```typescript
function removeDuplicateLettersImproved(s: string): string {
    const stack: string[] = [];
    const included = new Set<string>();
    const charFrequency = new Map<string, number>();
    
    // Calculate the frequency of each character.
    for (const char of s) {
        charFrequency.set(char, (charFrequency.get(char) || 0) + 1);
    }
    
    for (const char of s) {
        // Decrease the current character's frequency count.
        charFrequency.set(char, charFrequency.get(char)! - 1);
        
        // If character is already in the stack, skip it.
        if (included.has(char)) continue;
        
        // Maintain lexicographical order of the stack.
        while (stack.length > 0 && stack[stack.length - 1] > char && charFrequency.get(stack[stack.length - 1])! > 0) {
            const removedChar = stack.pop()!;
            included.delete(removedChar);
        }
        
        stack.push(char);
        included.add(char);
    }
    
    return stack.join('');
}
```

### Time Complexity:
- **O(n):** Each character in the string is pushed and can potentially be popped from the stack just once.
- **Space Complexity:** O(1), as the stack's length is bound to 26 distinct characters.

This structure ensures we achieve the smallest possible lexicographical result in an efficient manner.

