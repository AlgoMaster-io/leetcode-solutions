## [Leetcode 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

### Table of Contents:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Memoization (Top-Down DP)](#approach-2-memoization-top-down-dp)
- [Approach 3: Tabulation (Bottom-Up DP)](#approach-3-tabulation-bottom-up-dp)
- [Approach 4: Space-Optimized DP](#approach-4-space-optimized-dp) 

### Approach 1: Recursive Approach

#### Intuition:
The problem of finding the longest common subsequence can be solved by recursively determining if characters at current indices of both strings are equal. If they are, they contribute to a possible solution, and we move both indices. If they are different, we try both possibilities of skipping a character in one of the strings to check for a longer subsequence down the line. 

#### C# Code:
```csharp
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        return LCS(text1, text2, text1.Length, text2.Length);
    }
    
    private int LCS(string text1, string text2, int m, int n) {
        // Base case: if either string is exhausted, no common subsequence
        if (m == 0 || n == 0) {
            return 0;
        }
        
        // If last characters of both substrings match
        if (text1[m - 1] == text2[n - 1]) {
            return 1 + LCS(text1, text2, m - 1, n - 1);
        }
        // If last characters are not matching
        else {
            return Math.Max(LCS(text1, text2, m - 1, n), LCS(text1, text2, m, n - 1));
        }
    }
}
```

#### Time Complexity:
- The time complexity is O(2^(m+n)), where m and n are the lengths of the two input strings.
- This is because in the worst case, every character is compared with every other character, leading to an exponential number of possibilities.

#### Space Complexity:
- The space complexity is O(m + n) due to the recursive stack.

---

### Approach 2: Memoization (Top-Down DP)

#### Intuition:
The recursive approach recalculates solutions for overlapping subproblems. To eliminate this redundancy, store the computed results in a memoization table and reuse them.

#### C# Code:
```csharp
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        int[,] memo = new int[text1.Length + 1, text2.Length + 1];
        for (int i = 0; i <= text1.Length; i++) {
            for (int j = 0; j <= text2.Length; j++) {
                memo[i, j] = -1;
            }
        }
        return LCS(text1, text2, text1.Length, text2.Length, memo);
    }
    
    private int LCS(string text1, string text2, int m, int n, int[,] memo) {
        if (m == 0 || n == 0) {
            return 0;
        }
        
        if (memo[m, n] != -1) {
            return memo[m, n];
        }
        
        if (text1[m - 1] == text2[n - 1]) {
            memo[m, n] = 1 + LCS(text1, text2, m - 1, n - 1, memo);
        } else {
            memo[m, n] = Math.Max(LCS(text1, text2, m - 1, n, memo), LCS(text1, text2, m, n - 1, memo));
        }
        
        return memo[m, n];
    }
}
```

#### Time Complexity:
- The time complexity is O(m * n), where m and n are the lengths of the two strings.
- Each subproblem is computed only once and stored in the memo table.

#### Space Complexity:
- The space complexity is O(m * n) for the memoization table plus O(m + n) for the recursion stack.

---

### Approach 3: Tabulation (Bottom-Up DP)

#### Intuition:
This approach fills a table iteratively instead of using recursion and cache. Start from the base case and build solutions for larger strings using previously computed results.

#### C# Code:
```csharp
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        int m = text1.Length, n = text2.Length;
        int[,] dp = new int[m + 1, n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i, j] = dp[i - 1, j - 1] + 1;
                } else {
                    dp[i, j] = Math.Max(dp[i - 1, j], dp[i, j - 1]);
                }
            }
        }
        
        return dp[m, n];
    }
}
```

#### Time Complexity:
- The time complexity is O(m * n), processing each character of both strings to fill the table.

#### Space Complexity:
- The space complexity is O(m * n) for the DP table.

---

### Approach 4: Space-Optimized DP

#### Intuition:
Instead of using a full 2D table, the solution only relies on the current and previous rows in the computation, which allows using just two 1D arrays.

#### C# Code:
```csharp
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        int m = text1.Length, n = text2.Length;
        int[] previous = new int[n + 1];
        int[] current = new int[n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    current[j] = previous[j - 1] + 1;
                } else {
                    current[j] = Math.Max(current[j - 1], previous[j]);
                }
            }
            // Swap current and previous arrays for the next iteration
            int[] temp = previous;
            previous = current;
            current = temp;
        }
        
        return previous[n];
    }
}
```

#### Time Complexity:
- The time complexity remains O(m * n) due to the characters processing.

#### Space Complexity:
- The space complexity is O(n) for the additional arrays used to store the results.

---

Each approach progressively optimizes performance and memory usage from the recursive solution using memoization to a more optimal tabulated approach, eventually reducing space complexity with a time-efficient space-optimized DP method.

