# [Leetcode 1044: Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search with Rolling Hash & Rabin-Karp](#approach-2-binary-search-with-rolling-hash--rabin-karp)

### Approach 1: Brute Force

**Intuition:**

In this approach, we generate all possible substrings of a given length and check for duplicates. We start from substrings of length 1 to length `n-1` (where `n` is the length of the input string). We use a set to keep track of already seen substrings to efficiently find a duplicate.

**Algorithm:**

1. Iterate over possible substring lengths from 1 to `n-1`.
2. For each length, iterate over all possible starting positions to extract a substring of that length.
3. Use a set to track seen substrings and check if the current substring has been seen before.
4. If a duplicate is found, compare it against the longest duplicate found so far and update it if necessary.

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n^2)

**Code:**

```typescript
function longestDuplicateSubstring(s: string): string {
    const n = s.length;
    let longestDupSub = "";

    // Generate all possible substring lengths
    for (let length = 1; length < n; length++) {
        const seen = new Set<string>();

        // Slide over the entire string with the current length
        for (let start = 0; start <= n - length; start++) {
            const substring = s.substring(start, start + length);
            
            // If the substring is already seen, we have a duplicate
            if (seen.has(substring)) {
                if (substring.length > longestDupSub.length) {
                    longestDupSub = substring;
                }
            } else {
                seen.add(substring);
            }
        }
    }

    return longestDupSub;
}
```

---

### Approach 2: Binary Search with Rolling Hash & Rabin-Karp

**Intuition:**

Instead of checking all substring lengths, we can use binary search to efficiently find the maximum length of the duplicate substring. We combine this with a rolling hash (Rabin-Karp) to optimize the substring comparison process.

1. Use binary search to find the maximum possible length of a duplicate substring.
2. In each iteration, create a rolling hash for substrings of the currently considered length.
3. Use a set to store hash values. If a hash collision is found, re-check string segments to confirm that it isn't a false positive due to hash collisions.
4. Adjust binary search bounds based on whether a duplicate substring of the considered length is found.

**Time Complexity:** O(n log n)  
**Space Complexity:** O(n)

**Code:**

```typescript
function longestDuplicateSubstring(s: string): string {
    const MOD = 2 ** 31 - 1;
    let left = 1;
    let right = s.length - 1;
    let start: number = 0;

    const search = (length: number): number => {
        let hash = 0;
        const base = 256;  // Number of characters possible
        let powBaseLength = 1;
        const seenHashes = new Set<number>();

        for (let i = 0; i < length; i++) {
            hash = (hash * base + s.charCodeAt(i)) % MOD;
            powBaseLength = (powBaseLength * base) % MOD;
        }

        seenHashes.add(hash);

        for (let i = length; i < s.length; i++) {
            hash = (hash * base + s.charCodeAt(i)) % MOD;
            hash = (hash - (s.charCodeAt(i - length) * powBaseLength) % MOD + MOD) % MOD;
            
            if (seenHashes.has(hash)) return i - length + 1;
            seenHashes.add(hash);
        }
        
        return -1;
    };

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        const idx = search(mid);

        if (idx !== -1) {
            start = idx;
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return s.substring(start, start + left - 1);
}
```

Explanation:
- We utilize a rolling hash to perform an efficient check for duplicate substrings for each length by simulating a sliding window over the string.
- The hash value for substrings is calculated and used to identify potential duplicates without directly comparing string contents until a potential match is found.
- We adjust our binary search bounds based on whether a duplicate substring of the current middle length exists, refining our search to find the longest possible duplicate substring.

