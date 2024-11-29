# 518. [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approach 1: Recursive Approach

### Solution
python
```python
# Time Complexity: O(2^n), where n is the number of coins
# Space Complexity: O(n)
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        return self.countWays(coins, len(coins), amount)

    def countWays(self, coins: List[int], numOfCoins: int, amount: int) -> int:
        # Base case: If amount is 0, there is 1 solution (to use no coins)
        if amount == 0:
            return 1
        
        # If amount is less than 0, there's no solution
        if amount < 0:
            return 0
        
        # If there are no coins and amount is more than 0, no solution
        if numOfCoins <= 0 and amount > 0:
            return 0

        # Count is the sum of solutions including coins[numOfCoins-1]
        # and excluding coins[numOfCoins-1]
        return self.countWays(coins, numOfCoins - 1, amount) + self.countWays(coins, numOfCoins, amount - coins[numOfCoins - 1])
```

## Approach 2: Dynamic Programming (2D Array)

### Solution
python
```python
# Time Complexity: O(n * m), where n is the number of coins and m is the amount
# Space Complexity: O(n * m)
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # dp[i][j] means the number of ways to make change for amount j using first i types of coins
        dp = [[0] * (amount + 1) for _ in range(len(coins) + 1)]

        # Base case: If amount is 0, then there's exactly one way to make change: select no coin
        for i in range(len(coins) + 1):
            dp[i][0] = 1

        # Fill the dp array
        for i in range(1, len(coins) + 1):
            for j in range(1, amount + 1):
                # If we don't pick the coin, we have dp[i-1][j] possible ways
                dp[i][j] = dp[i - 1][j]

                # If we pick the coin, we add the ways we can make up the amount with this coin
                if j >= coins[i - 1]:
                    dp[i][j] += dp[i][j - coins[i - 1]]

        return dp[len(coins)][amount]
```

## Approach 3: Dynamic Programming (1D Array)

### Solution
python
```python
# Time Complexity: O(n * m), where n is the number of coins and m is the amount
# Space Complexity: O(m)
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # dp[j] means the number of ways to make change for amount j
        dp = [0] * (amount + 1)
        dp[0] = 1 # Base case: one way to make zero amount

        # Traverse each coin
        for coin in coins:
            # Update dp array by considering the current coin
            for j in range(coin, amount + 1):
                dp[j] += dp[j - coin]

        return dp[amount]
```


