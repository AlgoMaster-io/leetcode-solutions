# [LeetCode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Solutions

- [Solution 1: Brute Force](#solution-1-brute-force)
- [Solution 2: Sliding Window with Frequency Array](#solution-2-sliding-window-with-frequency-array)
- [Solution 3: Optimized Sliding Window](#solution-3-optimized-sliding-window)

### Solution 1: Brute Force

#### Intuition
The most straightforward way to solve this problem is to generate all possible substrings, then, for each substring, check how many replacements are required to make all characters in it the same. The answer will be the maximum length of such valid substrings.

```javascript
function characterReplacement(s, k) {
    let maxLength = 0;
    const n = s.length;

    // Try each possible substring
    for (let start = 0; start < n; start++) {
        for (let end = start; end < n; end++) {
            // Frequency map to count occurrences of each character
            let count = new Array(26).fill(0);
            let maxCharCount = 0;

            for (let i = start; i <= end; i++) {
                const index = s.charCodeAt(i) - 'A'.charCodeAt(0);
                count[index]++;
                maxCharCount = Math.max(maxCharCount, count[index]); // Keep track of most frequent char
            }

            // If it's possible to replace the rest of the characters to make a substring of same characters
            if (end - start + 1 - maxCharCount <= k) {
                maxLength = Math.max(maxLength, end - start + 1);
            }
        }
    }
    return maxLength;
}
```

**Time Complexity**: O(n^3) where n is the length of the string, due to both a double loop to generate substrings and an inner loop to count frequency.

**Space Complexity**: O(1) additional space is used aside from the frequency array of constant size.

### Solution 2: Sliding Window with Frequency Array

#### Intuition
The brute force solution is quite inefficient for long strings. We can use a sliding window approach to maintain and update the frequency count of characters as the window slides. This prevents us from recomputing counts from scratch for each window.

```javascript
function characterReplacement(s, k) {
    const n = s.length;
    let maxCharCount = 0, maxLength = 0;
    const count = new Array(26).fill(0);

    let left = 0;
    for (let right = 0; right < n; right++) {
        const index = s.charCodeAt(right) - 'A'.charCodeAt(0);
        count[index]++;
        maxCharCount = Math.max(maxCharCount, count[index]);

        // Calculate the current window size and check if it can be made uniform
        while (right - left + 1 - maxCharCount > k) {
            count[s.charCodeAt(left) - 'A'.charCodeAt(0)]--;
            left++;
        }

        maxLength = Math.max(maxLength, right - left + 1);
    }
    return maxLength;
}
```

**Time Complexity**: O(n) - The window slides across the string once.

**Space Complexity**: O(1) - Constant space for the frequency array.

### Solution 3: Optimized Sliding Window

#### Intuition
We can further improve the solution by maintaining only a single frequency tracker (`maxCharCount`) in the window while updating lengths and checking validity in a streamlined manner.

```javascript
function characterReplacement(s, k) {
    const n = s.length;
    let left = 0;
    let maxCharCount = 0; 
    let result = 0;
    const count = new Array(26).fill(0);

    for (let right = 0; right < n; right++) {
        const idx = s.charCodeAt(right) - 'A'.charCodeAt(0);
        count[idx]++;
        maxCharCount = Math.max(maxCharCount, count[idx]);

        // If the current window size minus the most frequent char count is bigger than k, it's not valid
        if (right - left + 1 - maxCharCount > k) {
            count[s.charCodeAt(left) - 'A'.charCodeAt(0)]--;
            left++;
        }

        result = Math.max(result, right - left + 1);
    }

    return result;
}
```

**Time Complexity**: O(n) - A single pass solution maintaining a sliding window.

**Space Complexity**: O(1) - Fixed space for a constant-size auxiliary array and counters.

