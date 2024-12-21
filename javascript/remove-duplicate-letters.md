# [Leetcode 316: Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approaches
- [Approach 1: Greedy Stack Approach](#approach-1-greedy-stack-approach)

---

### Approach 1: Greedy Stack Approach

In this approach, we aim to construct the smallest lexicographical order result while ensuring each letter appears only once. A stack data structure helps us maintain the best possible order of characters as we iterate through the input string.

#### Intuition:
1. As we iterate over each character, we decide either to include it in our stack/result or to ignore it if it has been included already.
2. For each character `ch`, if it is lexicographically smaller than the character at the top of the stack and the top character appears later (after the current index), we can pop the top character from the stack, making space for a better (smaller) character.
3. Utilize a "seen" set to ensure each character is only added to the stack once.

#### Steps:
1. Count occurrences of each character to determine if there is a possibility to push a character on the stack later if removed.
2. Iterate through each character in the string.
3. For each character, reduce its count from the map.
4. If it's already in the result, continue to the next character.
5. Check if popping the character from the result stack is beneficial:
   - Pop from the stack if the top character appears later in the string and the current character is smaller.
6. Finally, append the character to the result stack if it hasn't been seen.

#### Time Complexity:
The approach iterates over the string `s` twice, resulting in an O(n) complexity, where n is the length of the string.

#### Space Complexity:
The space for storing the result stack and seen characters can be O(k) in the worst case, where k is the number of unique characters.

```javascript
function removeDuplicateLetters(s) {
    // Create a frequency map to keep track of occurrences of each character
    const countMap = {};
    for (const char of s) {
        countMap[char] = (countMap[char] || 0) + 1;
    }

    // Stack to store the result characters
    const resultStack = [];
    // Set to keep track of characters we've added to resultStack
    const inResultStack = new Set();

    for (const char of s) {
        // Decrease the count for this character
        countMap[char]--;

        // If the character is already in resultStack, skip it
        if (inResultStack.has(char)) continue;

        // Remove characters from the stack while they:
        // - cause a lexicographically larger result
        // - still appear later in the string (so they can be re-added)
        while (resultStack.length > 0 && char < resultStack[resultStack.length - 1] && countMap[resultStack[resultStack.length - 1]] > 0) {
            const removedChar = resultStack.pop();
            inResultStack.delete(removedChar);
        }

        // Push the current character and mark it as included in resultStack
        resultStack.push(char);
        inResultStack.add(char);
    }

    // Join the stack to get the result string
    return resultStack.join('');
}
```

This approach efficiently constructs the smallest lexicographical string possible while ensuring each letter appears only once.

