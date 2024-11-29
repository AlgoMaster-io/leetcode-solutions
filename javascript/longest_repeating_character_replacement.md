# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
javascript
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function characterReplacement(s, k) {
    let maxLength = 0;

    // Iterate through every possible substring
    for (let i = 0; i < s.length; i++) {
        let freq = new Array(26).fill(0);
        let maxFreq = 0;

        for (let j = i; j < s.length; j++) {
            freq[s.charCodeAt(j) - 'A'.charCodeAt(0)]++;
            maxFreq = Math.max(maxFreq, freq[s.charCodeAt(j) - 'A'.charCodeAt(0)]);

            // Check if the current substring can be made uniform
            if ((j - i + 1) - maxFreq <= k) {
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }
    }

    return maxLength;
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function characterReplacement(s, k) {
    let freq = new Array(26).fill(0);
    let maxFreq = 0, maxLength = 0;
    let left = 0;

    // Sliding window
    for (let right = 0; right < s.length; right++) {
        freq[s.charCodeAt(right) - 'A'.charCodeAt(0)]++;
        maxFreq = Math.max(maxFreq, freq[s.charCodeAt(right) - 'A'.charCodeAt(0)]);

        // If the remaining characters exceed k, shrink the window
        while ((right - left + 1) - maxFreq > k) {
            freq[s.charCodeAt(left) - 'A'.charCodeAt(0)]--;
            left++;
        }

        // Update maxLength
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

