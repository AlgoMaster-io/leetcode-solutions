# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

## Solution

### Recursive Approach

#### Intuition
The naive recursive solution involves exploring all possible subsequences of the string to determine the longest palindromic subsequence. The basic idea is to use two pointers, one starting from the beginning of the string and one from the end. If the characters at these positions match, then include them in the palindrome and move both pointers inward. If they don't match, recursively solve for both possibilities by moving either pointer inward to find the longest subsequence.

#### Code
```java
public class Solution {
    public int longestPalindromeSubseq(String s) {
        return helper(s, 0, s.length() - 1);
    }
    
    private int helper(String s, int left, int right) {
        // Base case: if left exceeds right, return 0
        if (left > right) {
            return 0;
        }
        // Base case: if pointers meet, it's a palindrome of length 1
        if (left == right) {
            return 1;
        }
        // If characters at both positions match
        if (s.charAt(left) == s.charAt(right)) {
            return 2 + helper(s, left + 1, right - 1);
        }
        // Otherwise, check both possibilities and return the longer subsequence
        return Math.max(helper(s, left + 1, right), helper(s, left, right - 1));
    }
}
```

#### Complexity
- **Time Complexity:** O(2^n) due to exponential growth with each recursive call.
- **Space Complexity:** O(n) for the recursion stack depth.

### Memoization Approach

#### Intuition
To improve upon the recursive approach and avoid recalculating results for the same subproblems, memoization can be used. This involves caching results of previously computed states defined by their start and end indices.

#### Code
```java
public class Solution {
    public int longestPalindromeSubseq(String s) {
        int[][] memo = new int[s.length()][s.length()];
        return helper(s, 0, s.length() - 1, memo);
    }
    
    private int helper(String s, int left, int right, int[][] memo) {
        // Base case: if left exceeds right, return 0
        if (left > right) {
            return 0;
        }
        // Base case: if pointers meet, it's a palindrome of length 1
        if (left == right) {
            return 1;
        }
        // If this subproblem has already been solved, return the cached result
        if (memo[left][right] != 0) {
            return memo[left][right];
        }
        // If characters at both positions match
        if (s.charAt(left) == s.charAt(right)) {
            memo[left][right] = 2 + helper(s, left + 1, right - 1, memo);
        } else {
            // Otherwise, check both possibilities and cache the longer subsequence
            memo[left][right] = Math.max(helper(s, left + 1, right, memo), helper(s, left, right - 1, memo));
        }
        return memo[left][right];
    }
}
```

#### Complexity
- **Time Complexity:** O(n^2) due to caching results for all subproblems.
- **Space Complexity:** O(n^2) for the memoization table.

### Dynamic Programming Approach

#### Intuition
The most efficient way uses a bottom-up dynamic programming approach. We define a table `dp` where `dp[i][j]` represents the length of the longest palindromic subsequence between indices `i` and `j`. If the characters match, extend the subsequence by two; otherwise, choose the longest subsequence between [i+1, j] and [i, j-1].

#### Code
```java
public class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];

        // Single character palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // Check palindromes of increasing lengths
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;
                
                // If characters at start and end are the same
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                } else {
                    // Choose the longer subsequence after excluding each character
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        // The whole string result
        return dp[0][n - 1];
    }
}
```

#### Complexity
- **Time Complexity:** O(n^2) for filling the entire dp table.
- **Space Complexity:** O(n^2) for the dp table storage.

