# 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approach 1: Recursive Approach

### Solution
csharp
```csharp
// Time Complexity: Exponential
// Space Complexity: O(m + n) for the recursion stack
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        return Lcs(text1, text2, text1.Length, text2.Length);
    }
    
    private int Lcs(string s1, string s2, int m, int n) {
        // Base case: If either string is empty, return 0
        if (m == 0 || n == 0) {
            return 0;
        }
        
        // If the last characters match, reduce both lengths and add 1 to the result
        if (s1[m - 1] == s2[n - 1]) {
            return 1 + Lcs(s1, s2, m - 1, n - 1);
        } else {
            // Otherwise, take the maximum result by reducing one string at a time
            return Math.Max(Lcs(s1, s2, m - 1, n), Lcs(s1, s2, m, n - 1));
        }
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        int m = text1.Length;
        int n = text2.Length;
        int[,] dp = new int[m + 1, n + 1];

        // Fill the DP table iteratively
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    // Matching characters, take diagonal value and add 1
                    dp[i, j] = 1 + dp[i - 1, j - 1];
                } else {
                    // Different characters, take the maximum of left or top cell
                    dp[i, j] = Math.Max(dp[i - 1, j], dp[i, j - 1]);
                }
            }
        }

        // The bottom-right cell contains the length of LCS
        return dp[m, n];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(min(m, n))
public class Solution {
    public int LongestCommonSubsequence(string text1, string text2) {
        if (text1.Length < text2.Length) {
            return LongestCommonSubsequence(text2, text1); // Ensure text1 is larger or equal
        }
        
        int[] prev = new int[text2.Length + 1];

        // Iterate through each character of text1
        for (int i = 1; i <= text1.Length; i++) {
            int[] curr = new int[text2.Length + 1];

            // Iterate through each character of text2
            for (int j = 1; j <= text2.Length; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    curr[j] = 1 + prev[j - 1];
                } else {
                    curr[j] = Math.Max(curr[j - 1], prev[j]);
                }
            }
            prev = curr; // Move current row to previous for the next iteration
        }

        return prev[text2.Length]; // Last computed row contains the result
    }
}
```

