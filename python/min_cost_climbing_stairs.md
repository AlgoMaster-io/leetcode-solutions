# 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def minCostClimbingStairs(self, cost):
        memo = [-1] * len(cost)
        return min(self.minCost(len(cost) - 1, cost, memo),
                   self.minCost(len(cost) - 2, cost, memo))

    def minCost(self, i, cost, memo):
        if i < 0:
            return 0  # Base case: No cost for negative steps
        if memo[i] != -1:
            return memo[i]  # Return precomputed cost if available

        # Minimum cost to reach this step
        costToReach = cost[i] + min(self.minCost(i - 1, cost, memo), 
                                    self.minCost(i - 2, cost, memo))
        
        memo[i] = costToReach  # Store computed result in memo array
        return costToReach
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def minCostClimbingStairs(self, cost):
        n = len(cost)
        dp = [0] * (n + 1)

        # Base cases: No cost to start at either step 0 or step 1
        dp[0] = 0
        dp[1] = 0

        # Fill the dp array with minimum cost to reach each step
        for i in range(2, n + 1):
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
        
        return dp[n]  # Minimum cost to reach the top
```

## Approach 3: Dynamic Programming with Constant Space

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def minCostClimbingStairs(self, cost):
        n = len(cost)
        prev1, prev2 = 0, 0

        # Iterate through cost array and calculate minimum cost dynamically
        for i in range(2, n + 1):
            current = min(prev1 + cost[i - 1], prev2 + cost[i - 2])
            prev2 = prev1
            prev1 = current
        
        return prev1  # Minimum cost to reach the top
```

