# 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approach 1: Recursive Solution with Memoization

### Solution

```python
# Time Complexity: O(n^2)
# Space Complexity: O(n^2)
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        memo = [[0] * len(s) for _ in range(len(s))]
        return self.longestPalindromeSubseqHelper(s, 0, len(s) - 1, memo)
    
    def longestPalindromeSubseqHelper(self, s: str, start: int, end: int, memo: list) -> int:
        # Base case: single character is a palindrome
        if start == end:
            return 1
        # Base case: no characters (invalid scenario)
        if start > end:
            return 0
        # Check memo
        if memo[start][end] != 0:
            return memo[start][end]
        # If characters match, continue inward
        if s[start] == s[end]:
            memo[start][end] = 2 + self.longestPalindromeSubseqHelper(s, start + 1, end - 1, memo)
        else:
            # Explore both options and take the max
            memo[start][end] = max(self.longestPalindromeSubseqHelper(s, start + 1, end, memo),
                                   self.longestPalindromeSubseqHelper(s, start, end - 1, memo))
        return memo[start][end]
```

## Approach 2: Dynamic Programming

### Solution

```python
# Time Complexity: O(n^2)
# Space Complexity: O(n^2)
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0] * n for _ in range(n)]

        # Every single character is a palindrome of length 1
        for i in range(n):
            dp[i][i] = 1

        # Check subproblems of increasing lengths
        for length in range(2, n + 1):
            for start in range(n - length + 1):
                end = start + length - 1
                # Characters match, add 2 + result for inner substring
                if s[start] == s[end]:
                    dp[start][end] = dp[start + 1][end - 1] + 2
                else:
                    # Choose the best option from excluding either end
                    dp[start][end] = max(dp[start + 1][end], dp[start][end - 1])

        # Result is in dp[0][n-1]
        return dp[0][n - 1]
```

## Approach 3: Optimized Dynamic Programming with Reduced Space

### Solution

```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [0] * n
        prevDp = [0] * n

        # Every single character is a palindrome of length 1
        for i in range(n):
            dp[i] = 1

        # Check subproblems of increasing lengths
        for length in range(2, n + 1):
            for start in range(n - length + 1):
                end = start + length - 1
                temp = dp[start] # Preserve current value before overwriting

                if s[start] == s[end]:
                    dp[start] = prevDp[start + 1] + 2
                else:
                    dp[start] = max(dp[start + 1], prevDp[start])

                prevDp[start] = temp # Update prevDp for the next iteration

        # Result is in dp[0]
        return dp[0]
```

