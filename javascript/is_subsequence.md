# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function isSubsequence(s, t) {
    let sIndex = 0; // Pointer for string `s`
    let tIndex = 0; // Pointer for string `t`

    // Traverse both strings
    while (sIndex < s.length && tIndex < t.length) {
        if (s[sIndex] === t[tIndex]) {
            sIndex++; // Move pointer in `s` if characters match
        }
        tIndex++; // Always move pointer in `t`
    }

    // If sIndex has reached the end of `s`, it's a subsequence
    return sIndex === s.length;
}
```

## Approach 2: Iterative with Character Search

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function isSubsequence(s, t) {
    let index = -1; // Stores the last found position of characters

    // Iterate through each character in `s`
    for (const c of s) {
        // Find the next occurrence of `c` in `t` after the previous index
        index = t.indexOf(c, index + 1);
        if (index === -1) {
            // If character not found, `s` is not a subsequence of `t`
            return false;
        }
    }

    // All characters of `s` found in `t` in order
    return true;
}
```

## Approach 3: Binary Search (For Multiple Queries)

Use this approach when checking multiple subsequences against the same string `t`.

### Solution
```javascript
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
function isSubsequence(s, t) {
    // Preprocess `t`: Create a map of character positions
    const charPositions = new Map();
    for (let i = 0; i < t.length; i++) {
        if (!charPositions.has(t[i])) {
            charPositions.set(t[i], []);
        }
        charPositions.get(t[i]).push(i);
    }

    let lastPosition = -1; // Tracks the position of the last matched character

    // Check each character in `s`
    for (const c of s) {
        if (!charPositions.has(c)) {
            return false; // If `c` not in `t`, return false
        }

        // Perform binary search to find the next valid position
        const positions = charPositions.get(c);
        let nextPosition = binarySearch(positions, lastPosition + 1);

        if (nextPosition === positions.length) {
            return false; // No valid position found
        }

        lastPosition = positions[nextPosition]; // Update lastPosition
    }

    return true;
}

// Helper function for binary search
function binarySearch(arr, target) {
    let left = 0;
    let right = arr.length;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

