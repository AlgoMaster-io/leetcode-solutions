# [Leetcode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with HashMap](#approach-2-sliding-window-with-hashmap)
- [Approach 3: Optimized Sliding Window](#approach-3-optimized-sliding-window)

### Approach 1: Brute Force

In this approach, we will perform a simple brute force by checking every possible substring of the given string to find the longest substring which can be turned into one character by replacing at most 'k' characters.

#### Intuition
- Calculate all possible substrings.
- For each substring, check if it can be turned into a substring of repeating characters within the allowed 'k' replacements.
- Track the length of the longest valid substring.

#### Code
```typescript
function characterReplacement(s: string, k: number): number {
    const n = s.length;
    let maxLength = 0;

    // Generate all possible substrings
    for (let start = 0; start < n; start++) {
        for (let end = start; end < n; end++) {
            let freq = new Array(26).fill(0);
            let maxFreq = 0;
            // Calculate frequency of characters in the substring
            for (let i = start; i <= end; i++) {
                freq[s.charCodeAt(i) - 'A'.charCodeAt(0)]++;
                maxFreq = Math.max(maxFreq, freq[s.charCodeAt(i) - 'A'.charCodeAt(0)]);
            }
            // Condition to check if current substring can be made into one repeating character
            if ((end - start + 1) - maxFreq <= k) {
                maxLength = Math.max(maxLength, end - start + 1);
            }
        }
    }

    return maxLength;
}
```
#### Time Complexity
- **O(n^3)** due to generating and checking every substring.

#### Space Complexity
- **O(1)**, only constant extra space is used.

### Approach 2: Sliding Window with HashMap

Instead of considering all possible substrings, we can use a sliding window approach with a hashmap to store the frequency of each character within a window.

#### Intuition
- Use two pointers to define a window.
- Expand the window while maintaining a count of character frequencies.
- Calculate the current maximum frequency of any character in the window.
- If the number of characters that need to be replaced exceeds 'k', shrink the window from the left.

#### Code
```typescript
function characterReplacement(s: string, k: number): number {
    const n = s.length;
    let left = 0;
    let right = 0;
    let maxFreq = 0;
    let charCount = new Array(26).fill(0);
    let maxLength = 0;

    while (right < n) {
        const rightCharIndex = s.charCodeAt(right) - 'A'.charCodeAt(0);
        charCount[rightCharIndex]++;
        maxFreq = Math.max(maxFreq, charCount[rightCharIndex]);

        // Condition to check if more than 'k' characters need replacement
        while ((right - left + 1) - maxFreq > k) {
            const leftCharIndex = s.charCodeAt(left) - 'A'.charCodeAt(0);
            charCount[leftCharIndex]--;
            left++;
        }

        maxLength = Math.max(maxLength, right - left + 1);
        right++;
    }

    return maxLength;
}
```
#### Time Complexity
- **O(n)**, where `n` is the length of the string because each character is processed at most twice.

#### Space Complexity
- **O(1)**, since the hashmap size is constant (26 letters).

### Approach 3: Optimized Sliding Window

This is essentially the same as Approach 2, but explicitly keeps track of the window size, focusing on calculating the most efficient changes dynamically.

This approach has been implicitly covered in the structured solution above and typically suffices for performance optimization. Repeatedly updating the character count ensures that the implementation is concise and efficient.



