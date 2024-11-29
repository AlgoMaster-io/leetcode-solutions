# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function firstUniqChar(s) {
    const countMap = new Map();
    
    // Count the frequency of each character
    for (let c of s) {
        countMap.set(c, (countMap.get(c) || 0) + 1);
    }

    // Find the first character with frequency 1
    for (let i = 0; i < s.length; i++) {
        if (countMap.get(s[i]) === 1) {
            return i;
        }
    }

    return -1; // No unique character found
}
```

## Approach 2: Array for Character Frequency

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function firstUniqChar(s) {
    const charCount = new Array(26).fill(0); // Array to store frequency of each character

    // Count the frequency of each character
    for (let c of s) {
        charCount[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    // Find the first character with frequency 1
    for (let i = 0; i < s.length; i++) {
        if (charCount[s.charCodeAt(i) - 'a'.charCodeAt(0)] === 1) {
            return i;
        }
    }

    return -1; // No unique character found
}
```

## Approach 3: Single Pass with Index Array

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function firstUniqChar(s) {
    const index = new Array(26).fill(-1); // Array to track occurrences and order of characters
    
    // Traverse string to fill occurrences and order
    for (let i = 0; i < s.length; i++) {
        const charIndex = s.charCodeAt(i) - 'a'.charCodeAt(0);
        if (index[charIndex] === -1) {
            index[charIndex] = i; // First occurrence
        } else {
            index[charIndex] = -2; // Mark as repeated
        }
    }

    // Find the smallest index of a unique character
    let result = Infinity;
    for (let i = 0; i < 26; i++) {
        if (index[i] >= 0) {
            result = Math.min(result, index[i]);
        }
    }

    return result === Infinity ? -1 : result;
}
```

