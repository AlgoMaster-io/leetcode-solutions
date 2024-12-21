# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Solutions:

- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Space Optimized Dynamic Programming](#approach-4-space-optimized-dynamic-programming)

---

## Approach 1: Recursive Solution

### Intuition:

The problem can be broken down into subproblems using recursive thinking. We know that:
1. If the characters at the current indices are the same, they can be part of the longest palindromic subsequence.
2. If not the same, consider the maximum length by either excluding the current character from the start or the end.

Although this method will lead to a lot of redundant calculations, it forms the foundation for more optimized solutions.

### Code:

```csharp
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        return LongestPalindromeRecursive(s, 0, s.Length - 1);
    }
    
    private int LongestPalindromeRecursive(string s, int left, int right) {
        // Base Case: L and R cross each other
        if (left > right) return 0;
        
        // Base Case: Single character
        if (left == right) return 1;
        
        // If characters at current L and R match
        if (s[left] == s[right]) {
            return 2 + LongestPalindromeRecursive(s, left + 1, right - 1);
        } else {
            // If not, check both possibilities by removing either the left or right character
            return Math.Max(LongestPalindromeRecursive(s, left + 1, right),
                            LongestPalindromeRecursive(s, left, right - 1));
        }
    }
}
```

### Complexity:
- **Time Complexity:** O(2^N), where N is the length of the string `s`. This is due to the multiple overlapping subproblems.
- **Space Complexity:** O(N) for the recursion stack.

---

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition:

By adding memoization to the recursive approach, we store the results of previously solved subproblems. This eliminates redundant calculations and optimizes runtime.

### Code:

```csharp
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int[,] memo = new int[s.Length, s.Length];
        return LongestPalindromeMemo(s, 0, s.Length - 1, memo);
    }
    
    private int LongestPalindromeMemo(string s, int left, int right, int[,] memo) {
        if (left > right) return 0;
        if (left == right) return 1;
        if (memo[left, right] != 0) return memo[left, right];
        
        if (s[left] == s[right]) {
            memo[left, right] = 2 + LongestPalindromeMemo(s, left + 1, right - 1, memo);
        } else {
            memo[left, right] = Math.Max(LongestPalindromeMemo(s, left + 1, right, memo),
                                         LongestPalindromeMemo(s, left, right - 1, memo));
        }
        return memo[left, right];
    }
}
```

### Complexity:
- **Time Complexity:** O(N^2) due to caching of overlapping subproblems.
- **Space Complexity:** O(N^2) for the memoization table.

---

## Approach 3: Dynamic Programming (Bottom-Up)

### Intuition:

Using a DP table to store solutions to subproblems iteratively. Start by filling base cases (single characters) and then solve for longer substrings by building on smaller substrings.

### Code:

```csharp
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int n = s.Length;
        int[,] dp = new int[n, n];

        // Single characters are palindromes of length 1
        for (int i = 0; i < n; i++) {
            dp[i, i] = 1;
        }

        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;
                if (s[i] == s[j]) {
                    dp[i, j] = 2 + dp[i + 1, j - 1];
                } else {
                    dp[i, j] = Math.Max(dp[i + 1, j], dp[i, j - 1]);
                }
            }
        }

        return dp[0, n - 1];
    }
}
```

### Complexity:
- **Time Complexity:** O(N^2)
- **Space Complexity:** O(N^2)

---

## Approach 4: Space Optimized Dynamic Programming

### Intuition:

Further optimize space by only using the previous row of the DP table instead of the entire table, reducing the space complexity significantly.

### Code:

```csharp
public class Solution {
    public int LongestPalindromeSubseq(string s) {
        int n = s.Length;
        int[] dp = new int[n];

        for (int i = n - 1; i >= 0; i--) {
            int[] newDp = new int[n];
            newDp[i] = 1; // single character case
            for (int j = i + 1; j < n; j++) {
                if (s[i] == s[j]) {
                    newDp[j] = 2 + dp[j - 1];
                } else {
                    newDp[j] = Math.Max(dp[j], newDp[j - 1]);
                }
            }
            dp = newDp;
        }

        return dp[n - 1];
    }
}
```

### Complexity:
- **Time Complexity:** O(N^2)
- **Space Complexity:** O(N) due to optimization from using only one row at a time.

This concludes the detailed solution approaches for the problem of finding the longest palindromic subsequence. Each step builds upon the core recursive approach while optimizing for time and space.

