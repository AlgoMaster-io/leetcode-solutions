# [LeetCode 1044: Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Table of Contents

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Rabin-Karp with Binary Search](#approach-2-rabin-karp-with-binary-search)

## Approach 1: Brute Force

### Intuition

The brute force approach involves checking all possible substrings of the input string `s` to identify if there are any duplicates. We iterate over each substring length from 1 to `n-1` and check all possible starting positions for each length, determining whether any duplicate substrings exist.

### Steps

1. Iterate over possible lengths of substrings from 1 to `n-1`.
2. For each length, iterate over all possible starting positions and extract the substrings.
3. Use a `HashSet` to track the substrings seen so far for the current length.
4. If a substring is already in the `HashSet`, we have found a duplicate and can return it.

### Code

```csharp
public class Solution {
    public string LongestDupSubstring(string s) {
        int n = s.Length;
        // Initialize the longest duplicated substring
        string longestDup = string.Empty;
        
        // Iterate over possible lengths from longest down to 1
        for (int len = 1; len < n; len++) {
            // HashSet to store substrings of current length
            HashSet<string> seen = new HashSet<string>();
            // Flag to track if a duplicate was found for this length
            bool found = false;
            
            // Iterate over starting positions for substrings of length `len`
            for (int i = 0; i <= n - len; i++) {
                string sub = s.Substring(i, len);
                if (seen.Contains(sub)) {
                    found = true;
                    // Update the longest duplicated substring found so far
                    longestDup = sub;
                    break;
                }
                seen.Add(sub);
            }
            
            // If a duplicate was found, continue to search for longer duplicates
            if (!found) {
                break;
            }
        }
        
        return longestDup;
    }
}
```

### Complexity

- **Time Complexity:** \(O(n^3)\), where \(n\) is the length of the string. This is due to generating all substrings and checking for duplicates.
- **Space Complexity:** \(O(n^2)\), for storing substrings in the `HashSet`.

## Approach 2: Rabin-Karp with Binary Search

### Intuition

An efficient solution combines the Rabin-Karp algorithm for efficient substring hashing and binary search over possible substring lengths. Rabin-Karp helps in hashing the substrings to quickly check for duplicates, and binary search helps narrow down the length of the duplicate substring.

### Steps

1. Use binary search to check substring lengths: start with range `[0, n-1]`.
2. For each midpoint `mid` in the binary search, use Rabin-Karp to check if there's a duplicate substring of that length.
3. Adjust the binary search bounds based on the presence of duplicate substrings.
4. Return the longest duplicate substring found.

### Code

```csharp
public class Solution {
    public string LongestDupSubstring(string s) {
        int n = s.Length;
        int left = 1, right = n;
        string result = string.Empty;

        while (left < right) {
            int mid = left + (right - left) / 2;
            string dup = GetDuplicate(s, mid);

            if (dup != null) {
                result = dup;
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return result;
    }

    private string GetDuplicate(string s, int len) {
        // Base for Rabin-Karp
        long baseVal = 256;
        long mod = (long)Math.Pow(2, 32);

        long h = 0;
        long hash = 0;
        long baseP = 1;
        Dictionary<long, List<int>> seenHashes = new Dictionary<long, List<int>>();

        // Precompute the hash and the base power for fast computation
        for (int i = 0; i < len; i++) {
            hash = (hash * baseVal + s[i]) % mod;
            baseP = (baseP * baseVal) % mod;
        }

        seenHashes[hash] = new List<int> { 0 };

        for (int i = len; i < s.Length; i++) {
            hash = (hash * baseVal + s[i] - s[i - len] * baseP) % mod;
            if (hash < 0) {
                hash += mod; // Ensure hash is positive
            }

            if (seenHashes.TryGetValue(hash, out var indices)) {
                string substring = s.Substring(i - len + 1, len);
                foreach (int index in indices) {
                    if (s.Substring(index, len) == substring) {
                        return substring;
                    }
                }
            }
            
            if (!seenHashes.ContainsKey(hash)) {
                seenHashes[hash] = new List<int>();
            }
            seenHashes[hash].Add(i - len + 1);
        }
        return null;
    }
}
```

### Complexity

- **Time Complexity:** \(O(n \log n)\). Binary search over substring length involves \(O(\log n)\) steps, and each step is \(O(n)\) using Rabin-Karp.
- **Space Complexity:** \(O(n)\) for storing hashes.

