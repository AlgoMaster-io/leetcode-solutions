# [Leetcode 1044: Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Binary Search and Rolling Hash](#optimized-approach-using-binary-search-and-rolling-hash)

### Brute Force Approach

#### Intuition
The brute force approach is to generate all possible substrings and check if more than one instance of a substring exists. Although this will work eventually, it's far from optimal. Given the nature of substrings, we can sort them and then check adjacent pairs for duplicates.

The general idea is to consider substrings of all possible lengths, starting from the longest. If we find a duplicate substring of a certain length, we record that as our longest duplicate substring.

#### Implementation

```java
class Solution {
    public String longestDupSubstring(String s) {
        int n = s.length();
        int left = 0, right = n;
        String result = "";
        
        for (int len = right - 1; len > 0; len--) {
            Set<String> seen = new HashSet<>();
            boolean found = false;
            for (int i = 0; i <= n - len; i++) {
                String sub = s.substring(i, i + len);
                if (seen.contains(sub)) {
                    found = true;
                    result = sub;
                    break;
                }
                seen.add(sub);
            }
            if (found) break;
        }
        
        return result;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^3), where n is the length of the string. For each potential substring length, a substring comparison which in worst case can take O(n^2) time.
- **Space Complexity**: O(n^2), for storing all possible substrings.

### Optimized Approach using Binary Search and Rolling Hash

#### Intuition
The optimized approach uses binary search combined with a rolling hash technique to efficiently determine the longest duplicate substring.

1. **Binary Search**: We perform a binary search on the length of the substring. Initially, set `left` as 1 and `right` as n (length of string).
2. **Rolling Hash**: Pre-calculate the hash values for substrings to avoid recalculating hash values for overlapping sequences. This can be efficiently achieved using powers of a base number and modulus operations.

This method narrows down to the longest possible duplicate substring by leveraging the logarithmic exploration of the binary search and the efficiency of rolling hash.

#### Implementation

```java
class Solution {
    private static final long MOD = 2_000_000_000;

    public String longestDupSubstring(String s) {
        int n = s.length();
        int left = 1, right = n;
        String result = "";

        while (left < right) {
            int mid = left + (right - left) / 2;
            String dup = getDuplicateSubstring(s, mid);
            if (dup != null) {
                result = dup;
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return result;
    }

    private String getDuplicateSubstring(String s, int len) {
        int n = s.length();
        long base = 256;  // Larger alphabet size for ASCII characters
        long hash = 0;
        long pow = 1;
        Map<Long, List<Integer>> seen = new HashMap<>();

        // Calculate the hash of the first 'len' characters
        for (int i = 0; i < len; i++) {
            hash = (hash * base + s.charAt(i)) % MOD;
            pow = (pow * base) % MOD;
        }

        seen.putIfAbsent(hash, new ArrayList<>());
        seen.get(hash).add(0);

        // Roll the hash over the string
        for (int i = len; i < n; i++) {
            hash = (hash * base + s.charAt(i) - s.charAt(i - len) * pow % MOD + MOD) % MOD;
            if (seen.containsKey(hash)) {
                // Check if the actual substrings match (prevent potential hash collision)
                for (int start : seen.get(hash)) {
                    if (s.substring(start, start + len).equals(s.substring(i - len + 1, i + 1))) {
                        return s.substring(start, start + len);
                    }
                }
            }
            seen.putIfAbsent(hash, new ArrayList<>());
            seen.get(hash).add(i - len + 1);
        }

        return null;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n log n), where n is the length of the string. Binary search contributes a `log n` factor, and each call to check for duplicates using rolling hash is O(n).
- **Space Complexity**: O(n), for storing hash values and their start positions in the hashmap.

This approach is significantly more efficient, allowing you to tackle inputs at the scale imposed by typical constraints in competitive programming and interview settings.

