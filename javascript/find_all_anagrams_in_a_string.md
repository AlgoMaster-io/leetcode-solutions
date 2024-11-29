# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
javascript
```javascript
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array

var findAnagrams = function(s, p) {
    const result = [];
    const pFreq = new Array(26).fill(0);

    // Frequency count for string p
    for (let char of p) {
        pFreq[char.charCodeAt() - 'a'.charCodeAt()]++;
    }

    // Check every substring of length p in s
    for (let i = 0; i <= s.length - p.length; i++) {
        const sFreq = new Array(26).fill(0);

        // Count frequency of the current substring
        for (let j = i; j < i + p.length; j++) {
            sFreq[s[j].charCodeAt() - 'a'.charCodeAt()]++;
        }

        // Compare frequencies
        if (pFreq.every((val, index) => val === sFreq[index])) {
            result.push(i);
        }
    }

    return result;
};
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
javascript
```javascript
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)

var findAnagrams = function(s, p) {
    const result = [];
    const pFreq = new Array(26).fill(0);
    const sFreq = new Array(26).fill(0);

    // Frequency count for string p
    for (let char of p) {
        pFreq[char.charCodeAt() - 'a'.charCodeAt()]++;
    }

    let left = 0, right = 0;

    // Sliding window over s
    while (right < s.length) {
        // Add the current character to the window
        sFreq[s[right].charCodeAt() - 'a'.charCodeAt()]++;

        // Check if the window size matches p
        if (right - left + 1 === p.length) {
            // Compare frequencies
            if (pFreq.every((val, index) => val === sFreq[index])) {
                result.push(left);
            }

            // Remove the leftmost character from the window
            sFreq[s[left].charCodeAt() - 'a'.charCodeAt()]--;
            left++;
        }

        right++;
    }

    return result;
};
```

