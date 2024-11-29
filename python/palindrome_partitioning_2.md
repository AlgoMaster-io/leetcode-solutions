# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n^2)
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        is_palindrome = [[False] * n for _ in range(n)]
        
        # Fill the is_palindrome table
        for length in range(1, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1
                if s[i] == s[j]:
                    is_palindrome[i][j] = length <= 2 or is_palindrome[i + 1][j - 1]
        
        cuts = [0] * n
        for i in range(n):
            if is_palindrome[0][i]:
                cuts[i] = 0
            else:
                cuts[i] = i
                for j in range(i):
                    if is_palindrome[j + 1][i]:
                        cuts[i] = min(cuts[i], cuts[j] + 1)
        
        return cuts[n - 1]
```

## Approach 2: Optimized with Less Space for Palindrome Check

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        cuts = [0] * n
        is_palindrome = [False] * n
        
        for i in range(n):
            min_cuts = i
            for j in range(i + 1):
                if s[j] == s[i] and (i - j <= 1 or is_palindrome[j + 1]):
                    is_palindrome[j] = True
                    min_cuts = min(min_cuts, 0 if j == 0 else cuts[j - 1] + 1)
                else:
                    is_palindrome[j] = False
            cuts[i] = min_cuts
        
        return cuts[n - 1]
```

