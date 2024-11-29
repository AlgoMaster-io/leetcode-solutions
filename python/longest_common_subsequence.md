# 1143. [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approach 1: Recursive Approach

### Solution
python
```python
# Time Complexity: Exponential
# Space Complexity: O(m + n) for the recursion stack
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        return self.lcs(text1, text2, len(text1), len(text2))
    
    def lcs(self, s1: str, s2: str, m: int, n: int) -> int:
        # Base case: If either string is empty, return 0
        if m == 0 or n == 0:
            return 0
        
        # If the last characters match, reduce both lengths and add 1 to the result
        if s1[m - 1] == s2[n - 1]:
            return 1 + self.lcs(s1, s2, m - 1, n - 1)
        else:
            # Otherwise, take the maximum result by reducing one string at a time
            return max(self.lcs(s1, s2, m - 1, n), self.lcs(s1, s2, m, n - 1))
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m * n)
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # Fill the DP table iteratively
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    # Matching characters, take diagonal value and add 1
                    dp[i][j] = 1 + dp[i - 1][j - 1]
                else:
                    # Different characters, take the maximum of left or top cell
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        # The bottom-right cell contains the length of LCS
        return dp[m][n]
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(min(m, n))
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        if len(text1) < len(text2):
            return self.longestCommonSubsequence(text2, text1) # Ensure text1 is larger or equal
        
        prev = [0] * (len(text2) + 1)

        # Iterate through each character of text1
        for i in range(1, len(text1) + 1):
            curr = [0] * (len(text2) + 1)

            # Iterate through each character of text2
            for j in range(1, len(text2) + 1):
                if text1[i - 1] == text2[j - 1]:
                    curr[j] = 1 + prev[j - 1]
                else:
                    curr[j] = max(curr[j - 1], prev[j])
            prev = curr # Move current row to previous for the next iteration

        return prev[len(text2)] # Last computed row contains the result
```

