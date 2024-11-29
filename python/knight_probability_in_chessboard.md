# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n^2 * k)
# Space Complexity: O(n^2 * k)
class Solution:
    DIRECTIONS = [
        (1, 2), (1, -2), (-1, 2), (-1, -2),
        (2, 1), (2, -1), (-2, 1), (-2, -1)
    ]
    
    def knightProbability(self, N: int, K: int, r: int, c: int) -> float:
        memo = [[[None for _ in range(K + 1)] for _ in range(N)] for _ in range(N)]
        return self.findProbability(N, K, r, c, memo) / (8 ** K)
    
    def findProbability(self, N: int, K: int, r: int, c: int, memo) -> float:
        if r < 0 or r >= N or c < 0 or c >= N:
            return 0
        if K == 0:
            return 1
        if memo[r][c][K] is not None:
            return memo[r][c][K]
        
        prob = 0
        for dr, dc in self.DIRECTIONS:
            prob += self.findProbability(N, K - 1, r + dr, c + dc, memo)
        
        memo[r][c][K] = prob
        return prob
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
python
```python
# Time Complexity: O(n^2 * k)
# Space Complexity: O(n^2)
class Solution:
    DIRECTIONS = [
        (1, 2), (1, -2), (-1, 2), (-1, -2),
        (2, 1), (2, -1), (-2, 1), (-2, -1)
    ]

    def knightProbability(self, N: int, K: int, r: int, c: int) -> float:
        dp0 = [[0] * N for _ in range(N)]
        dp0[r][c] = 1

        for step in range(K):
            dp1 = [[0] * N for _ in range(N)]
            for i in range(N):
                for j in range(N):
                    if dp0[i][j] > 0:
                        for dr, dc in self.DIRECTIONS:
                            ni, nj = i + dr, j + dc
                            if 0 <= ni < N and 0 <= nj < N:
                                dp1[ni][nj] += dp0[i][j] / 8.0
            dp0 = dp1

        return sum(map(sum, dp0))
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n^2 * k)
# Space Complexity: O(n^2)
class Solution:
    DIRECTIONS = [
        (1, 2), (1, -2), (-1, 2), (-1, -2),
        (2, 1), (2, -1), (-2, 1), (-2, -1)
    ]

    def knightProbability(self, N: int, K: int, r: int, c: int) -> float:
        current = [[0] * N for _ in range(N)]
        current[r][c] = 1

        for step in range(K):
            next = [[0] * N for _ in range(N)]
            for i in range(N):
                for j in range(N):
                    if current[i][j] > 0:
                        for dr, dc in self.DIRECTIONS:
                            ni, nj = i + dr, j + dc
                            if 0 <= ni < N and 0 <= nj < N:
                                next[ni][nj] += current[i][j] / 8.0
            current = next

        return sum(map(sum, current))
```

