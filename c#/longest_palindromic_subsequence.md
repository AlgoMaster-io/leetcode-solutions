# 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approach 1: Recursive Solution with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int[,] memo = new int[s.Length, s.Length];
        return LongestPalindromeSubseqHelper(s, 0, s.Length - 1, memo);
    }
    
    private int LongestPalindromeSubseqHelper(string s, int start, int end, int[,] memo) {
        // Base case: single character is a palindrome
        if (start == end) {
            return 1;
        }
        // Base case: no characters (invalid scenario)
        if (start > end) {
            return 0;
        }
        // Check memo
        if (memo[start, end] != 0) {
            return memo[start, end];
        }
        // If characters match, continue inward
        if (s[start] == s[end]) {
            memo[start, end] = 2 + LongestPalindromeSubseqHelper(s, start + 1, end - 1, memo);
        } else {
            // Explore both options and take the max
            memo[start, end] = Math.Max(LongestPalindromeSubseqHelper(s, start + 1, end, memo),
                                        LongestPalindromeSubseqHelper(s, start, end - 1, memo));
        }
        return memo[start, end];
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int n = s.Length;
        int[,] dp = new int[n, n];

        // Every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) {
            dp[i, i] = 1;
        }

        // Check subproblems of increasing lengths
        for (int length = 2; length <= n; length++) {
            for (int start = 0; start <= n - length; start++) {
                int end = start + length - 1;
                // Characters match, add 2 + result for inner substring
                if (s[start] == s[end]) {
                    dp[start, end] = dp[start + 1, end - 1] + 2;
                } else {
                    // Choose the best option from excluding either end
                    dp[start, end] = Math.Max(dp[start + 1, end], dp[start, end - 1]);
                }
            }
        }

        // Result is in dp[0,n - 1]
        return dp[0, n - 1];
    }
}
```

## Approach 3: Optimized Dynamic Programming with Reduced Space

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int n = s.Length;
        int[] dp = new int[n];
        int[] prevDp = new int[n];

        // Every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
        }

        // Check subproblems of increasing lengths
        for (int length = 2; length <= n; length++) {
            for (int start = 0; start <= n - length; start++) {
                int end = start + length - 1;
                int temp = dp[start]; // Preserve current value before overwriting

                if (s[start] == s[end]) {
                    dp[start] = prevDp[start + 1] + 2;
                } else {
                    dp[start] = Math.Max(dp[start + 1], prevDp[start]);
                }

                prevDp[start] = temp; // Update prevDp for the next iteration
            }
        }

        // Result is in dp[0]
        return dp[0];
    }
}
```

