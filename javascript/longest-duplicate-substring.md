# Leetcode Problem: [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Approaches
1. [Brute Force Approach](#1-brute-force-approach)
2. [Binary Search with Rabin-Karp and Rolling Hashes](#2-binary-search-with-rabin-karp-and-rolling-hashes)

---

## 1. Brute Force Approach

### Intuition
The simplest way to find the longest duplicate substring is to consider all possible substrings of given string and check if there is more than one occurrence of the substring in the string. This is not efficient and can be very slow for larger strings.

### Code
```javascript
function longestDupSubstring(s) {
    const n = s.length;

    // Helper function to check if a given substring exists more than once
    const hasDuplicate = (substring) => {
        let count = 0;
        for (let i = 0; i <= n - substring.length; i++) {
            if (s.substring(i, i + substring.length) === substring) {
                count++;
                if (count > 1) return true;
            }
        }
        return false;
    };

    let longestSub = '';

    for (let len = 1; len < n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const sub = s.substring(i, i + len);
            if (hasDuplicate(sub) && sub.length > longestSub.length) {
                longestSub = sub;
            }
        }
    }

    return longestSub;
}
```
### Time Complexity
- Worst-case time complexity is O(n^3), where n is the length of the string. This comes from two nested loops to generate substrings and another loop in the `hasDuplicate` function.

### Space Complexity
- O(1), only using a few extra variables regardless of input size.

---

## 2. Binary Search with Rabin-Karp and Rolling Hashes

### Intuition
A more efficient way combines binary search and the Rabin-Karp algorithm. The central idea is to use binary search to find the maximum length L of a duplicate substring, and use a rolling hash to check for possible duplicate substrings of a certain length.

### Code
```javascript
function longestDupSubstring(s) {
    const n = s.length;
    const MOD = 2 ** 63 - 1;

    const search = (length) => {
        const base = 256;
        let currentHash = 0;
        const seenHashes = new Set();

        for (let i = 0; i < length; i++) {
            currentHash = (currentHash * base + s.charCodeAt(i)) % MOD;
        }

        seenHashes.add(currentHash);

        let baseL = 1;
        for (let i = 1; i < length; i++) {
            baseL = (baseL * base) % MOD;
        }

        for (let i = length; i < n; i++) {
            currentHash = (currentHash * base + s.charCodeAt(i) - s.charCodeAt(i - length) * baseL) % MOD;
            currentHash = (currentHash + MOD) % MOD; // Ensure currentHash isn't negative

            if (seenHashes.has(currentHash)) {
                return i - length + 1;
            }
            seenHashes.add(currentHash);
        }

        return -1;
    };

    let lo = 1, hi = n, start = -1;
    while (lo < hi) {
        const mid = Math.floor((lo + hi) / 2);
        const pos = search(mid);
        if (pos !== -1) {
            lo = mid + 1;
            start = pos;
        } else {
            hi = mid;
        }
    }

    return start !== -1 ? s.substring(start, start + lo - 1) : '';
}
```
### Time Complexity
- The expected time complexity is O(n log n), where n is the length of the string. The binary search contributes the log n factor, and using a rolling hash contributes an O(n) factor for each hash length check.

### Space Complexity
- O(n), primarily for the set of seen hashes, which in worst case could store up to n hashes.

