# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n * m)
# Space Complexity: O(n * m)
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        # Memoization table
        memo = [[-1 for _ in range(len(t))] for _ in range(len(s))]
        return self.numDistinctHelper(s, t, 0, 0, memo)
    
    def numDistinctHelper(self, s: str, t: str, sIndex: int, tIndex: int, memo: list) -> int:
        # If t is exhausted, one subsequence is found
        if tIndex == len(t):
            return 1
        # If s is exhausted first, no subsequence can be formed
        if sIndex == len(s):
            return 0
        # Check memoization table
        if memo[sIndex][tIndex] != -1:
            return memo[sIndex][tIndex]
        
        # If characters match, both decisions can be made
        if s[sIndex] == t[tIndex]:
            memo[sIndex][tIndex] = self.numDistinctHelper(s, t, sIndex + 1, tIndex + 1, memo) + \
                                   self.numDistinctHelper(s, t, sIndex + 1, tIndex, memo)
        else:
            # If characters don't match, skip the current character in s
            memo[sIndex][tIndex] = self.numDistinctHelper(s, t, sIndex + 1, tIndex, memo)
        
        return memo[sIndex][tIndex]
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n * m)
# Space Complexity: O(n * m)
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        # DP table
        dp = [[0] * (len(t) + 1) for _ in range(len(s) + 1)]

        # Base case: empty t can be formed by all possible substrings of s
        for i in range(len(s) + 1):
            dp[i][len(t)] = 1

        # Fill the table in reverse order
        for i in range(len(s) - 1, -1, -1):
            for j in range(len(t) - 1, -1, -1):
                if s[i] == t[j]:
                    # Both using the character and ignoring it
                    dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j]
                else:
                    # Ignore the character in s
                    dp[i][j] = dp[i + 1][j]

        # Result is the number of ways to form t[0...m] from s[0...n]
        return dp[0][0]
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
python
```python
# Time Complexity: O(n * m)
# Space Complexity: O(m)
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        # Previous and current row for space optimization
        prev = [0] * (len(t) + 1)
        curr = [0] * (len(t) + 1)
        
        # Base case: empty t can be formed by all possible substrings of s
        for i in range(len(s) + 1):
            prev[len(t)] = 1
        
        # Fill the table in reverse order
        for i in range(len(s) - 1, -1, -1):
            for j in range(len(t) - 1, -1, -1):
                if s[i] == t[j]:
                    curr[j] = prev[j + 1] + prev[j]
                else:
                    curr[j] = prev[j]
            # Move current row to previous for next iteration
            prev = curr[:]
        
        # Result is the number of ways to form t[0...m] from s[0...n]
        return prev[0]
```

