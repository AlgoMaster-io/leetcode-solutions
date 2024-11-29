# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def __init__(self):
        self.memo = {}

    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
        if n in self.memo:
            return self.memo[n]

        # Recurrence relation: f(n) = f(n-1) + f(n-2)
        result = self.climbStairs(n - 1) + self.climbStairs(n - 2)
        self.memo[n] = result  # Store the result in a map to avoid recalculating
        return result
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2

        # Build the dp array with previous solutions
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[n]  # The answer is stored in the last element
```

## Approach 3: Optimized Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        first, second = 1, 2  # Represents dp[i-2] and dp[i-1], respectively

        # Use two variables to avoid full dp array and update them iteratively
        for i in range(3, n + 1):
            current = first + second  # Current step number is the sum of the previous two steps
            first = second  # Move second to first
            second = current  # Update second to the current

        return current  # Result is stored in current
```

