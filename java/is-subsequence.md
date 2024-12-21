## [Leetcode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

### Approaches
- [Approach 1: Two Pointers](#approach-1-two-pointers)
- [Approach 2: Dynamic Programming (DP)](#approach-2-dynamic-programming-dp)

---

### Approach 1: Two Pointers

**Intuition**:  
The simplest way to determine if a string `s` is a subsequence of `t` is to iterate over the string `t` while trying to match characters with `s`. We use two pointers, one for each string. Start with both pointers at the beginning. If the characters at both pointers match, move the pointer of the subsequence `s`. Always move the pointer for `t`. If we can move through all characters of `s` and each character matches as we iterate through `t`, then `s` is a subsequence of `t`.

1. Initialize two pointers, `i` for `s` and `j` for `t`.
2. Iterate over `t` using `j` and check if `s[i]` matches `t[j]`.
3. If they match, increment `i`.
4. If `i` reaches the length of `s`, it means all characters in `s` have been matched correctly in order within `t`.
5. Return true if `i` equals the length of `s`, meaning `s` is a subsequence. Otherwise, return false.

```java
public class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0, j = 0;
        // Iterate through both strings
        while (i < s.length() && j < t.length()) {
            // If characters match, move the pointer for s
            if (s.charAt(i) == t.charAt(j)) {
                i++;
            }
            // Always move the pointer for t
            j++;
        }
        // If i is equal to the length of s, all characters of s were found in t
        return i == s.length();
    }
}
```

- **Time Complexity**: O(n + m), where n is the length of `s` and m is the length of `t`.
- **Space Complexity**: O(1), no additional space is used.

---

### Approach 2: Dynamic Programming (DP)

**Intuition**:  
Dynamic Programming can be used to solve the problem by finding the Longest Common Subsequence (LCS) between `s` and `t`. If the length of LCS (s, t) is equal to the length of `s`, then `s` is a subsequence of `t`.

1. Create a 2D DP array `dp` where `dp[i][j]` will represent the length of the LCS of `s[0..i-1]` and `t[0..j-1]`.
2. Initialize `dp[0][j]` and `dp[i][0]` to 0 because an empty string has a LCS of 0 with any string.
3. Fill the DP table:
   - If `s[i-1] == t[j-1]`, then `dp[i][j] = dp[i-1][j-1] + 1`.
   - Otherwise, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
4. Finally, check if `dp[n][m]` (where n is `s.length()` and m is `t.length()`) is equal to `s.length()`.

```java
public class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = s.length();
        int m = t.length();
        // Create dp array
        int[][] dp = new int[n + 1][m + 1];
        
        // Fill DP array
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        // Check if LCS length is equal to the length of s
        return dp[n][m] == n;
    }
}
```

- **Time Complexity**: O(n * m), where n is the length of `s` and m is the length of `t`.
- **Space Complexity**: O(n * m), due to storing intermediate LCS lengths in a 2D table.

---

By analyzing the time complexities, the Two Pointers approach is most efficient for this problem due to its simplicity and linear complexity, making it the ideal choice for large input sizes. The DP solution, while more complex, offers a clear path for understanding subsequence relationships and can be extended to more complex variants of subsequences problems.

