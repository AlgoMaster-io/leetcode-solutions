# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n^2)

class Solution:
    def __init__(self):
        self.memo = {}

    def soupServings(self, N):
        if N >= 5000:
            return 1.0  # Threshold optimization
        return self.serve(N, N)

    def serve(self, a, b):
        if a <= 0 and b <= 0:
            return 0.5  # Both soups are finished
        if a <= 0:
            return 1  # Only soup A finishes
        if b <= 0:
            return 0  # Only soup B finishes

        key = (a, b)
        if key in self.memo:
            return self.memo[key]

        probability = 0.25 * (
            self.serve(a - 100, b) +
            self.serve(a - 75, b - 25) +
            self.serve(a - 50, b - 50) +
            self.serve(a - 25, b - 75)
        )

        self.memo[key] = probability
        return probability
```

## Approach 2: Iterative with Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n^2)

class Solution:
    def soupServings(self, N):
        if N >= 5000:
            return 1.0  # Threshold optimization
        N = (N + 24) // 25  # Reduce and round up

        dp = [[0] * (N + 1) for _ in range(N + 1)]
        dp[0][0] = 0.5  # Base case: both soups are empty
        for i in range(1, N + 1):
            dp[i][0] = 0  # Soup B empty first
            dp[0][i] = 1  # Soup A empty first

        for i in range(1, N + 1):
            for j in range(1, N + 1):
                dp[i][j] = 0.25 * (
                    self.getProbability(dp, i - 4, j) +
                    self.getProbability(dp, i - 3, j - 1) +
                    self.getProbability(dp, i - 2, j - 2) +
                    self.getProbability(dp, i - 1, j - 3)
                )

        return dp[N][N]

    def getProbability(self, dp, a, b):
        if a <= 0 and b <= 0:
            return 0.5  # Both finish at the same time
        if a <= 0:
            return 1  # Only soup A finishes
        if b <= 0:
            return 0  # Only soup B finishes
        return dp[a][b]
```

