# [LeetCode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Navigation
- [Approach 1: Two-Pointer Technique](#approach-1-two-pointer-technique)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Two-Pointer Technique

### Intuition
The idea is to use two pointers to traverse both the sequence `s` and string `t`. We will try to match characters from `s` with `t` in order. If we can match all characters of `s` using `t`, then `s` is a subsequence of `t`.

### Solution

```csharp
public class Solution {
    public bool IsSubsequence(string s, string t) {
        int sPointer = 0, tPointer = 0;
        
        // Traverse both the strings with the help of two pointers.
        while (sPointer < s.Length && tPointer < t.Length) {
            // If characters match, move the pointer of 's'
            if (s[sPointer] == t[tPointer]) {
                sPointer++;
            }
            // Always move the pointer of 't'
            tPointer++;
        }
        
        // If we've managed to match all characters of 's', sPointer reaches s.Length
        return sPointer == s.Length;
    }
}
```

### Complexity
- **Time Complexity**: O(n + m), where n is the length of `s` and m is the length of `t`, because we potentially iterate through each character of both strings once.
- **Space Complexity**: O(1), constant space as no extra data structures were used.

---

## Approach 2: Dynamic Programming

### Intuition
For each position in `t`, we store the next occurrence of each character from 'a' to 'z'. We can then iterate through characters in `s` using this pre-computed information to efficiently find the next character in `t`. This is more optimal when `s` is checked against `t` multiple times.

### Solution

```csharp
public class Solution {
    public bool IsSubsequence(string s, string t) {
        // Initialize dp table where dp[i][j] represents the next position of character j (0-25 for 'a'-'z') after index i in t
        int[][] dp = new int[t.Length + 1][];
        for (int i = 0; i <= t.Length; i++) {
            dp[i] = new int[26];
            Array.Fill(dp[i], -1);
        }

        // Fill the table in reverse order
        for (int i = t.Length - 1; i >= 0; i--) {
            for (int j = 0; j < 26; j++) {
                dp[i][j] = dp[i + 1][j];
            }
            dp[i][t[i] - 'a'] = i;
        }

        // Now use the dp table to check if s is a subsequence
        int currentIndex = 0;
        for (int i = 0; i < s.Length; i++) {
            if (dp[currentIndex][s[i] - 'a'] == -1) {
                return false;
            }
            currentIndex = dp[currentIndex][s[i] - 'a'] + 1;
        }
        
        return true;
    }
}
```

### Complexity
- **Time Complexity**: O(m + s*m) in preprocessing, where m is the length of `t`, and then O(n) for each check where n is the length of `s`.
- **Space Complexity**: O(m * 26) to store the dp array.

