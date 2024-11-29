# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
python
```python
# Time Complexity: O(S^n) where S is the amount and n is the number of coins
# Space Complexity: O(S) for the recursion stack

class Solution:
    def coinChange(self, coins, amount):
        if amount == 0:
            return 0
        result = self.helper(coins, amount)
        return -1 if result == float('inf') else result

    def helper(self, coins, remainder):
        if remainder < 0:
            return float('inf')
        if remainder == 0:
            return 0

        min_coins = float('inf')
        for coin in coins:
            res = self.helper(coins, remainder - coin)
            
            # Avoid integer overflow here and take minimum
            if res != float('inf'):
                min_coins = min(min_coins, res + 1)
        return min_coins
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
python
```python
# Time Complexity: O(S * n)
# Space Complexity: O(S)

class Solution:
    def coinChange(self, coins, amount):
        dp = [amount + 1] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0:
                    dp[i] = min(dp[i], dp[i - coin] + 1)
        
        # If dp[amount] is still amount + 1, no solution found
        return -1 if dp[amount] > amount else dp[amount]
```

## Approach 3: Dynamic Programming - Top Down with Memoization

### Solution
python
```python
# Time Complexity: O(S * n)
# Space Complexity: O(S)

class Solution:
    def __init__(self):
        self.memo = []

    def coinChange(self, coins, amount):
        self.memo = [-1] * (amount + 1)
        self.memo[0] = 0
        return self.coinChangeRecursive(coins, amount)

    def coinChangeRecursive(self, coins, remaining):
        if remaining < 0:
            return -1
        if self.memo[remaining] != -1:
            return self.memo[remaining]

        min_coins = float('inf')
        for coin in coins:
            res = self.coinChangeRecursive(coins, remaining - coin)
            
            if res >= 0:
                min_coins = min(min_coins, res + 1)

        self.memo[remaining] = -1 if min_coins == float('inf') else min_coins
        return self.memo[remaining]
```


