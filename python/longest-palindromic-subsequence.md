[Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)
- [Approach 3: Space Optimized Dynamic Programming](#approach-3-space-optimized-dynamic-programming)

### Approach 1: Recursion with Memoization

**Intuition:**

The longest palindromic subsequence can be found using a recursive approach where, at each step, we check if the first and last characters are the same. If they are, they form part of the palindrome, and we recursively call for the inner substring. If they are not the same, we check both possibilities by adding either the first or the last character and take the maximum result of the two recursive calls. To optimize, we will use memoization to avoid redundant calculations.

**Code:**

```python
def longestPalindromeSubseq(s: str) -> int:
    # Memoization table
    memo = {}

    def helper(l, r):
        if l > r:
            return 0
        if l == r:
            return 1

        if (l, r) in memo:
            return memo[(l, r)]

        if s[l] == s[r]:
            memo[(l, r)] = 2 + helper(l + 1, r - 1)
        else:
            memo[(l, r)] = max(helper(l + 1, r), helper(l, r - 1))

        return memo[(l, r)]
    
    return helper(0, len(s) - 1)
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n^2) due to the recursion stack and memoization table.

---

### Approach 2: Dynamic Programming (Bottom-Up)

**Intuition:**

Using a bottom-up dynamic programming approach, we build a 2D DP table where `dp[i][j]` represents the longest palindromic subsequence in the substring `s[i..j]`. The table is filled based on previously computed subproblems, leveraging the relationships defined in the recursive approach.

**Code:**

```python
def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]

    for i in range(n - 1, -1, -1):
        dp[i][i] = 1  # each character is a palindrome of length 1
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = 2 + dp[i + 1][j - 1]
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

    return dp[0][n - 1]
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n^2) for the DP table.

---

### Approach 3: Space Optimized Dynamic Programming

**Intuition:**

Optimizing the space, we realize to compute `dp[i][j]`, only `dp[i+1][...]` (the current row) and `dp[i][...]` (the previous row) are needed. Thus, we can reduce the space complexity to O(n) by only keeping track of the current and previous rows.

**Code:**

```python
def longestPalindromeSubseq(s: str) -> int:
    n = len(s)
    prev = [0] * n

    for i in range(n - 1, -1, -1):
        current = [0] * n
        current[i] = 1  # each character is a palindrome of length 1
        for j in range(i + 1, n):
            if s[i] == s[j]:
                current[j] = 2 + prev[j - 1]
            else:
                current[j] = max(prev[j], current[j - 1])
        prev = current

    return prev[n - 1]
```

**Time Complexity:** O(n^2)  
**Space Complexity:** O(n) reduced from O(n^2).

