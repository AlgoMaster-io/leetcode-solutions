# [Problem: Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approaches
- [Approach 1: Horizontal Scanning](#approach-1-horizontal-scanning)
- [Approach 2: Vertical Scanning](#approach-2-vertical-scanning)
- [Approach 3: Divide and Conquer](#approach-3-divide-and-conquer)
- [Approach 4: Binary Search](#approach-4-binary-search)

## Approach 1: Horizontal Scanning

### Intuition
The goal is to find the longest common prefix string among an array of strings. The idea with Horizontal Scanning is to take the first string as a reference and compare it character by character with the remaining strings to find the common prefix.

### Steps
1. Initialize the prefix as the first string.
2. Iterate through the remaining strings:
   - Update the prefix by comparing with each string, character by character.
   - Truncate the prefix where a mismatch is found.
3. If at any point the prefix becomes empty, return an empty string as the common prefix.

### Code
```typescript
function longestCommonPrefix(strs: string[]): string {
    if (strs.length === 0) return "";
    
    let prefix = strs[0];
    
    for (let i = 1; i < strs.length; i++) {
        // Compare the prefix with the current string
        while (strs[i].indexOf(prefix) !== 0) {
            // Truncate the last character from prefix
            prefix = prefix.slice(0, prefix.length - 1);
            // If prefix becomes empty, return ""
            if (prefix === "") return "";
        }
    }
    
    return prefix;
}
```

### Complexity
- **Time Complexity:** O(N * M), where N is the number of strings, and M is the length of the first string (worst-case scenario). Each comparison in `indexOf` takes O(M) time.
- **Space Complexity:** O(1), only extra space used is for the prefix string.

## Approach 2: Vertical Scanning

### Intuition
In Vertical Scanning, instead of comparing string by string, we compare character by character at each index.

### Steps
1. Iterate over the characters of the first string.
2. For each character position, check all strings to see if they all have the same character at that position.
3. If a mismatch is found or the end of any string is reached, return the common prefix found so far.

### Code
```typescript
function longestCommonPrefixVertical(strs: string[]): string {
    if (strs.length === 0) return "";
    
    for (let i = 0; i < strs[0].length; i++) {
        const char = strs[0][i];
        for (let j = 1; j < strs.length; j++) {
            // If this character is not present in a string or does not match the char from first string
            if (i >= strs[j].length || strs[j][i] !== char) {
                return strs[0].slice(0, i);
            }
        }
    }
    
    return strs[0];
}
```

### Complexity
- **Time Complexity:** O(N * M), where N is the number of strings, and M is the minimum length of the strings. We compare each character of each string.
- **Space Complexity:** O(1), as no extra space is used.

## Approach 3: Divide and Conquer

### Intuition
This approach applies the divide and conquer technique to recursively find the longest common prefix between two halves of the list, then combine the results.

### Steps
1. Divide the array of strings into two halves.
2. Recursively find the longest common prefix for each half.
3. Find the common prefix for the two halves by character comparison.

### Code
```typescript
function longestCommonPrefixDivideAndConquer(strs: string[]): string {
    if (strs.length === 0) return "";
    return longestCommonPrefixDC(strs, 0, strs.length - 1);
}

function longestCommonPrefixDC(strs: string[], left: number, right: number): string {
    if (left === right) {
        return strs[left];
    } else {
        const mid = Math.floor((left + right) / 2);
        const leftLCP = longestCommonPrefixDC(strs, left, mid);
        const rightLCP = longestCommonPrefixDC(strs, mid + 1, right);
        return commonPrefix(leftLCP, rightLCP);
    }
}

function commonPrefix(left: string, right: string): string {
    const minLength = Math.min(left.length, right.length);
    for (let i = 0; i < minLength; i++) {
        if (left[i] !== right[i]) {
            return left.slice(0, i);
        }
    }
    return left.slice(0, minLength);
}
```

### Complexity
- **Time Complexity:** O(N * M), where N is the number of strings and M is the minimum string length due to repeated combining of string results.
- **Space Complexity:** O(M * log N) due to recursive call stack space.

## Approach 4: Binary Search

### Intuition
We can consider the problem as searching for the maximum prefix length that is common in all strings. We use binary search to efficiently find this length.

### Steps
1. Determine the smallest string length as the upper bound for binary search.
2. Use binary search to find the maximum length of the prefix common in all strings.
3. Validate the lengths using a helper function.

### Code
```typescript
function longestCommonPrefixBinarySearch(strs: string[]): string {
    if (strs.length === 0) return "";
    
    let minLen = Math.min(...strs.map(s => s.length));
    let low = 1;
    let high = minLen;
    
    while (low <= high) {
        const middle = Math.floor((low + high) / 2);
        if (isCommonPrefix(strs, middle)) {
            low = middle + 1;
        } else {
            high = middle - 1;
        }
    }
    
    return strs[0].slice(0, (low + high) / 2);
}

function isCommonPrefix(strs: string[], length: number): boolean {
    const prefix = strs[0].slice(0, length);
    for (const s of strs) {
        if (!s.startsWith(prefix)) {
            return false;
        }
    }
    return true;
}
```

### Complexity
- **Time Complexity:** O(N * M * log M), where N is the number of strings and M is the minimum string length. Each iteration checks if the first `middle` characters are common, which is O(N * M).
- **Space Complexity:** O(1), no additional space is used.

