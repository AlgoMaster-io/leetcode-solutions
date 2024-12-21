# [Leetcode 1525: Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized with Prefix and Suffix Arrays](#approach-2-optimized-with-prefix-and-suffix-arrays)

### Approach 1: Brute Force

**Intuition:**

In this approach, we aim for a straightforward method by trying to split the string at every possible position and then count the distinct characters on both sides to decide if it's a valid split.

**Steps:**

1. Traverse the string and for every index, attempt to split the string into two parts.
2. For each split, calculate the number of distinct characters in the left part and the right part.
3. Compare the counts of distinct characters on both sides. Increment the valid split counter if they are equal.

**Time Complexity:** O(n^2) - For each of the n possible splits, we count distinct characters, which could take O(n).

**Space Complexity:** O(n) - The space required to store distinct characters could be O(n) in the worst case.

```typescript
function numSplits(s: string): number {
    // Helper function to calculate the count of distinct characters in a substring
    function countDistinctCharacters(sub: string): number {
        const charSet = new Set(sub);
        return charSet.size;
    }

    let goodSplits = 0;

    // Try every possible split
    for (let i = 1; i < s.length; i++) {
        const leftPart = s.substring(0, i);
        const rightPart = s.substring(i);
        
        const leftDistinctCount = countDistinctCharacters(leftPart);
        const rightDistinctCount = countDistinctCharacters(rightPart);

        // Increment goodSplits if distinct counts on both sides are equal
        if (leftDistinctCount === rightDistinctCount) {
            goodSplits++;
        }
    }

    return goodSplits;
}
```

### Approach 2: Optimized with Prefix and Suffix Arrays

**Intuition:**

Instead of calculating the distinct elements for both parts separately for each split, use prefix and suffix data structures to calculate the count of distinct elements more efficiently.

**Steps:**

1. Traverse the string once to fill prefix distinct count array where `prefix[i]` represents the number of distinct characters from the start to index `i`.
2. Traverse the string again to fill suffix distinct count array where `suffix[i]` represents the number of distinct characters from index `i` to the end.
3. For each possible split, compare the prefix and suffix values to determine if it's a valid split.

**Time Complexity:** O(n) - Each pass over the string to fill the prefix and suffix arrays is linear.

**Space Complexity:** O(n) - We maintain two arrays to store prefix and suffix distinct character counts.

```typescript
function numSplits(s: string): number {
    const n = s.length;
    const prefix = new Array(n).fill(0);
    const suffix = new Array(n).fill(0);
    
    const prefixSet = new Set<string>();
    const suffixSet = new Set<string>();
    
    // Calculate prefix array
    for (let i = 0; i < n; i++) {
        prefixSet.add(s[i]);
        prefix[i] = prefixSet.size;
    }
    
    // Calculate suffix array
    for (let i = n - 1; i >= 0; i--) {
        suffixSet.add(s[i]);
        suffix[i] = suffixSet.size;
    }
    
    let goodSplits = 0;
    // Check for each split position
    for (let i = 0; i < n - 1; i++) {
        if (prefix[i] === suffix[i + 1]) {
            goodSplits++;
        }
    }
    
    return goodSplits;
}
```

In this approach, by leveraging prefix and suffix arrays, we significantly reduce the computational overhead and achieve an O(n) solution, making it apt for handling larger strings efficiently.

