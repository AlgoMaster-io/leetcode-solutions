# [Leetcode 44: Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

## Approaches
1. [Recursive Backtracking](#recursive-backtracking)
2. [Dynamic Programming 2D Array](#dynamic-programming-2d-array)
3. [Dynamic Programming Optimized Space](#dynamic-programming-optimized-space)

---

### Recursive Backtracking

**Intuition**:
The basic approach to this problem is to use recursion for matching the pattern with the string. We'll handle each character in the string and pattern one by one. The pattern can have '*' and '?' which need to be specially handled:
- '?' matches any single character.
- '*' matches any sequence of characters (including empty), which suggests trying multiple breakpoints in the string for this case.

**Code**:
```csharp
public class Solution {
    public bool IsMatch(string s, string p) {
        return IsMatchRecursive(s, p, 0, 0);
    }

    private bool IsMatchRecursive(string s, string p, int i, int j) {
        // If both strings are completely traversed
        if (i == s.Length && j == p.Length) return true;
        // If pattern is exhausted but string remains
        if (j == p.Length) return false;
        
        // If string is exhausted, handle only remaining '*'
        if (i == s.Length) {
            while (j < p.Length && p[j] == '*') j++;
            return j == p.Length;
        }

        if (p[j] == '?' || p[j] == s[i]) {
            // Current characters match or pattern has '?'
            return IsMatchRecursive(s, p, i + 1, j + 1);
        }

        if (p[j] == '*') {
            // '*' can represent an empty sequence or different length sequences
            return IsMatchRecursive(s, p, i, j + 1) || IsMatchRecursive(s, p, i + 1, j);
        }

        return false;
    }
}
```

**Complexity**:
- Time Complexity: O(2^(n+m)), since each '*' can branch to match or not match a character.
- Space Complexity: O(n + m), to handle recursive stack space.

---

### Dynamic Programming 2D Array

**Intuition**:
Dynamic programming is used here to store results of computations that have been done before, thus avoiding recomputation and improving performance. We will use a 2D array `dp` where `dp[i][j]` will be true if `s[0...i-1]` matches `p[0...j-1]`.

**Code**:
```csharp
public class Solution {
    public bool IsMatch(string s, string p) {
        int m = s.Length, n = p.Length;
        bool[,] dp = new bool[m + 1, n + 1];
        
        dp[0, 0] = true;  // Both string and pattern are empty
        
        // Only '*' can match an empty string
        for (int j = 1; j <= n; j++) {
            if (p[j - 1] == '*') {
                dp[0, j] = dp[0, j - 1];
            }
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j - 1] == '*') {
                    dp[i, j] = dp[i - 1, j] || dp[i, j - 1];
                } else if (p[j - 1] == '?' || s[i - 1] == p[j - 1]) {
                    dp[i, j] = dp[i - 1, j - 1];
                }
            }
        }
        
        return dp[m, n];
    }
}
```

**Complexity**:
- Time Complexity: O(n*m), where n and m are the lengths of the string and pattern.
- Space Complexity: O(n*m), for the 2D dp array.

---

### Dynamic Programming Optimized Space

**Intuition**:
We can optimize the space complexity further since each state in the dp array depends only on the current and previous rows. Hence, we can use two 1D arrays to track the current and previous results, reducing space usage.

**Code**:
```csharp
public class Solution {
    public bool IsMatch(string s, string p) {
        int m = s.Length, n = p.Length;
        bool[] previous = new bool[n + 1];
        bool[] current = new bool[n + 1];
        
        // Initialize base case
        previous[0] = true;
        for (int j = 1; j <= n; j++) {
            if (p[j - 1] == '*') {
                previous[j] = previous[j - 1];
            }
        }
        
        for (int i = 1; i <= m; i++) {
            current[0] = false;
            for (int j = 1; j <= n; j++) {
                if (p[j - 1] == '*') {
                    current[j] = previous[j] || current[j - 1];
                } else if (p[j - 1] == '?' || s[i - 1] == p[j - 1]) {
                    current[j] = previous[j - 1];
                } else {
                    current[j] = false;
                }
            }
            // Move current row to previous row for next iteration
            var temp = previous;
            previous = current;
            current = temp;
        }
        
        return previous[n];
    }
}
```

**Complexity**:
- Time Complexity: O(n*m), same as the 2D DP approach.
- Space Complexity: O(n), using only two 1D arrays for storage.

