# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
```javascript
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
function longestCommonPrefix(strs) {
    if (!strs || strs.length === 0) {
        return "";
    }

    // Start with the first string as the prefix
    let prefix = strs[0];

    // Compare the prefix with each string in the array
    for (let i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) !== 0) {
            // Shorten the prefix until it matches the start of the current string
            prefix = prefix.substring(0, prefix.length - 1);
            if (prefix === "") {
                return ""; // No common prefix
            }
        }
    }

    return prefix;
}
```

## Approach 2: Vertical Scanning

### Solution
```javascript
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
function longestCommonPrefix(strs) {
    if (!strs || strs.length === 0) {
        return "";
    }

    // Iterate over each character of the first string
    for (let i = 0; i < strs[0].length; i++) {
        const c = strs[0][i];

        // Compare this character with the same position in all strings
        for (let j = 1; j < strs.length; j++) {
            // If characters don't match or exceed the string's length
            if (i >= strs[j].length || strs[j][i] !== c) {
                return strs[0].substring(0, i); // Return the common prefix
            }
        }
    }

    return strs[0]; // The entire first string is the common prefix
}
```

## Approach 3: Divide and Conquer

### Solution
```javascript
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(m * log n) where m is the average string length
function longestCommonPrefix(strs) {
    if (!strs || strs.length === 0) {
        return "";
    }

    return longestCommonPrefixHelper(strs, 0, strs.length - 1);
}

function longestCommonPrefixHelper(strs, left, right) {
    if (left === right) {
        return strs[left]; // Base case: single string
    }

    // Divide the array into two halves
    const mid = Math.floor((left + right) / 2);
    const lcpLeft = longestCommonPrefixHelper(strs, left, mid);
    const lcpRight = longestCommonPrefixHelper(strs, mid + 1, right);

    // Merge results
    return commonPrefix(lcpLeft, lcpRight);
}

function commonPrefix(left, right) {
    const minLength = Math.min(left.length, right.length);
    for (let i = 0; i < minLength; i++) {
        if (left[i] !== right[i]) {
            return left.substring(0, i);
        }
    }
    return left.substring(0, minLength);
}
```

## Approach 4: Binary Search

### Solution
```javascript
// Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
// Space Complexity: O(1)
function longestCommonPrefix(strs) {
    if (!strs || strs.length === 0) {
        return "";
    }

    let minLen = Infinity;
    for (const str of strs) {
        minLen = Math.min(minLen, str.length);
    }

    let low = 1, high = minLen;
    while (low <= high) {
        const mid = Math.floor((low + high) / 2);
        if (isCommonPrefix(strs, mid)) {
            low = mid + 1; // Try for a longer prefix
        } else {
            high = mid - 1; // Try for a shorter prefix
        }
    }

    return strs[0].substring(0, Math.floor((low + high) / 2));
}

function isCommonPrefix(strs, len) {
    const prefix = strs[0].substring(0, len);
    for (let i = 1; i < strs.length; i++) {
        if (!strs[i].startsWith(prefix)) {
            return false;
        }
    }
    return true;
}
```

