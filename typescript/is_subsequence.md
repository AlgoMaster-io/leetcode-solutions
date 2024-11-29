# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
export function isSubsequence(s: string, t: string): boolean {
    let sIndex = 0; // Pointer for string `s`
    let tIndex = 0; // Pointer for string `t`

    // Traverse both strings
    while (sIndex < s.length && tIndex < t.length) {
        if (s.charAt(sIndex) === t.charAt(tIndex)) {
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
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
export function isSubsequence(s: string, t: string): boolean {
    let index = -1; // Stores the last found position of characters

    // Iterate through each character in `s`
    for (let c of s) {
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

Use this approach when checking multiple subsequences against the same string t.

### Solution
typescript
```typescript
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
export function isSubsequence(s: string, t: string): boolean {
    const charPositions: Map<string, number[]> = new Map();

    // Preprocess `t`: Create a map of character positions
    for (let i = 0; i < t.length; i++) {
        const char = t.charAt(i);
        if (!charPositions.has(char)) {
            charPositions.set(char, []);
        }
        charPositions.get(char)!.push(i);
    }

    let lastPosition = -1; // Tracks the position of the last matched character

    // Check each character in `s`
    for (let c of s) {
        if (!charPositions.has(c)) {
            return false; // If `c` not in `t`, return false
        }

        const positions = charPositions.get(c)!;
        
        // Perform binary search to find the next valid position
        let nextPosition = binarySearch(positions, lastPosition + 1);
        if (nextPosition === positions.length) {
            return false; // No valid position found
        }

        lastPosition = positions[nextPosition]; // Update lastPosition
    }

    return true;
}

// Helper function: Perform binary search
function binarySearch(arr: number[], target: number): number {
    let low = 0;
    let high = arr.length;

    while (low < high) {
        const mid = Math.floor((low + high) / 2);
        if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }

    return low;
}
```

