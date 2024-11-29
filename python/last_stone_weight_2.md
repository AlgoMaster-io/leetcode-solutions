# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
```python
# Time Complexity: O(n * S), where S is the sum of all stones
# Space Complexity: O(S)
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp = {0}
        total_sum = sum(stones)
        
        # Iterate over each stone
        for stone in stones:
            new_dp = set()
            # Update possible sums with and without the current stone
            for s in dp:
                new_dp.add(s + stone)
                new_dp.add(s - stone)
            # Move to the new set of possible sums
            dp = new_dp

        # Find the minimum possible non-negative sum (half the total sum)
        min_abs = float('inf')
        for s in dp:
            if s >= 0:
                min_abs = min(min_abs, s)
        
        return min_abs
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
```python
# Time Complexity: O(n * S), where S is the sum of all stones
# Space Complexity: O(S)
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        total_sum = sum(stones)
        dp = [False] * (total_sum // 2 + 1)
        dp[0] = True

        # Evaluate possible subset sums
        for stone in stones:
            for j in range(len(dp) - 1, stone - 1, -1):
                dp[j] = dp[j] or dp[j - stone]

        # Find the largest possible subset sum that is <= total_sum/2
        for j in range(len(dp) - 1, -1, -1):
            if dp[j]:
                return total_sum - 2 * j

        return 0
```

## Approach 3: Optimized DP with Single Array

### Solution
```python
# Time Complexity: O(n * S), where S is the sum of all stones
# Space Complexity: O(S)
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        total_sum = sum(stones)
        target = total_sum // 2
        dp = [0] * (target + 1)

        # Calculate optimized subset sums
        for stone in stones:
            for j in range(target, stone - 1, -1):
                dp[j] = max(dp[j], dp[j - stone] + stone)

        # Result is the difference between total sum and twice the best subset sum
        return total_sum - 2 * dp[target]
```


