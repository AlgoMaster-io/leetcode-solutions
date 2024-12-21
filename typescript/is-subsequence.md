# [Leetcode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approaches
1. [Two Pointers Approach](#two-pointers-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)

### Two Pointers Approach

**Intuition:** 

The idea is to maintain two pointers, one for the sequence "s" and another for "t". We start checking the characters of "s" one by one in "t", advancing both pointers whenever we find a matching character. If we manage to traverse all characters in "s" by the time our "t" pointer reaches the end, then "s" is a subsequence of "t".

This approach leverages the ordered and linear structure of both "s" and "t" to verify subsequence nature without the necessity of backtracking or excessive memory usage.

**Time Complexity:** O(n) - where n is the length of string "t".

**Space Complexity:** O(1)

```typescript
function isSubsequence(s: string, t: string): boolean {
  let i = 0, j = 0;
  
  // Traverse through string t
  while (i < s.length && j < t.length) {
    // If characters match, move i pointer to the next character in s
    if (s[i] === t[j]) {
      i++;
    }
    // Always move j pointer to check the next character in t
    j++;
  }

  // If we have traversed all characters of s, return true
  return i === s.length;
}
```

### Dynamic Programming Approach

**Intuition:**

For "is subsequence" problems that require handling multiple tasks efficiently, a dynamic programming approach can improve performance. By creating a table (or DP array) where we precompute the positions of characters from `t`, we can quickly check subsequence membership.

We precompute the next occurrence index for each character in `t` at each position. With this in place, we can determine if any subsequent character in the sequence "s" is present in "t" by looking up our table.

**Time Complexity:** O(n + m) - where n is the length of string "t" and m is the length of string "s".

**Space Complexity:** O(n)

```typescript
function isSubsequenceDP(s: string, t: string): boolean {
  const n = t.length;
  const charIndexMap: Map<string, number[]> = new Map();

  // Create mapping for each character and its indices in string t
  for (let i = 0; i < n; i++) {
    if (!charIndexMap.has(t[i])) {
      charIndexMap.set(t[i], []);
    }
    charIndexMap.get(t[i])!.push(i);
  }

  let currentIndex = -1; // Start from position -1

  for (const char of s) {
    if (!charIndexMap.has(char)) {
      return false; // If character from s is not in t, return false
    }

    // Get list of indices for `char` and find the next viable index
    let indices = charIndexMap.get(char)!;
    let nextIndex = findNextIndex(indices, currentIndex);

    if (nextIndex === -1) {
      return false; // No valid index found, hence not a subsequence
    }

    currentIndex = nextIndex; // Update the current index
  }

  return true;
}

// Helper function to find the next index in indices array using binary search
function findNextIndex(indices: number[], currentIndex: number): number {
  let low = 0;
  let high = indices.length;
  while (low < high) {
    let mid = Math.floor((low + high) / 2);
    if (indices[mid] <= currentIndex) {
      low = mid + 1;
    } else {
      high = mid;
    }
  }
  return low < indices.length ? indices[low] : -1;
}
```

The dynamic programming approach optimizes multiple queries about subsequences in the string "t" by leveraging precomputed data. This method enhances efficiency especially when dealing with repeated checks.

