# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n * sqrt(n))
# Space Complexity: O(n)
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0  # Zero can be represented by zero squares
        
        # Start filling dp array
        for i in range(1, n + 1):
            for j in range(1, int(i**0.5) + 1):
                dp[i] = min(dp[i], dp[i - j*j] + 1)
        
        return dp[n]  # Result is stored in dp[n]
```

## Approach 2: Breadth-First Search (BFS)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def numSquares(self, n: int) -> int:
        queue = deque([0])
        visited = [0] * (n + 1)
        visited[0] = 1
        
        level = 0
        while queue:
            size = len(queue)
            level += 1
            for _ in range(size):
                sum_ = queue.popleft()
                for j in range(1, int(n**0.5) + 1):
                    next_sum = sum_ + j * j
                    if next_sum == n:
                        return level
                    if next_sum <= n and visited[next_sum] == 0:
                        queue.append(next_sum)
                        visited[next_sum] = 1
        
        return level
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
python
```python
# Time Complexity: O(sqrt(n))
# Space Complexity: O(1)
import math

class Solution:
    def numSquares(self, n: int) -> int:
        # Check if n is a perfect square
        if self.isPerfectSquare(n):
            return 1
        
        # Check the result is 4 using Legendre's Theorem
        while n % 4 == 0: 
            n //= 4
        if n % 8 == 7:
            return 4
        
        # Check if it can be decomposed into sum of two squares
        for i in range(1, int(n**0.5) + 1):
            if self.isPerfectSquare(n - i*i):
                return 2
        
        return 3
    
    def isPerfectSquare(self, n: int) -> bool:
        s = int(math.sqrt(n))
        return s * s == n
```

