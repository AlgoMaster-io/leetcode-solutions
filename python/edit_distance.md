# 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approach 1: Recursive Solution

### Solution
python
```python
# Time Complexity: O(3^(m+n))
# Space Complexity: O(m+n)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        # Helper method to compute edit distance recursively
        def computeDistance(m, n):
            # If first string is empty, return length of second string
            if m == 0:
                return n
            
            # If second string is empty, return length of first string
            if n == 0:
                return m

            # If characters are the same, continue with next characters
            if word1[m - 1] == word2[n - 1]:
                return computeDistance(m - 1, n - 1)

            # Calculate minimum operation from insert, remove, or replace
            return 1 + min(
                computeDistance(m, n - 1),    # Insert
                min(
                    computeDistance(m - 1, n),    # Remove
                    computeDistance(m - 1, n - 1) # Replace
                )
            )
        
        return computeDistance(len(word1), len(word2))
```

## Approach 2: Dynamic Programming (2D Table)

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m * n)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)
        
        # Create DP table
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # Initialize base cases
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j

        # Fill in DP table
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] # Characters match, no new operation
                else:
                    dp[i][j] = 1 + min(
                        dp[i - 1][j],    # Remove
                        min(
                            dp[i][j - 1],    # Insert
                            dp[i - 1][j - 1] # Replace
                        )
                    )

        return dp[m][n]
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(n)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)
        
        # Create two arrays for dynamic programming, only need two rows at a time
        prev = list(range(n + 1))
        curr = [0] * (n + 1)

        # Fill in DP table
        for i in range(1, m + 1):
            curr[0] = i # Base case for the first column
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    curr[j] = prev[j - 1] # Characters match
                else:
                    curr[j] = 1 + min(
                        prev[j],    # Remove
                        min(
                            curr[j - 1],    # Insert
                            prev[j - 1]     # Replace
                        )
                    )
            prev, curr = curr, prev

        return prev[n]
```

