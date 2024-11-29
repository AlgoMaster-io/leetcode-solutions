# 123. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approach 1: Dynamic Programming with State Variables

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

class Solution:
    def maxProfit(self, prices):
        hold1 = float('-inf')
        hold2 = float('-inf')
        release1 = 0
        release2 = 0

        for price in prices:
            # First transaction
            release1 = max(release1, hold1 + price)  # Sell stock held by hold1
            hold1 = max(hold1, -price)               # Buy stock

            # Second transaction
            release2 = max(release2, hold2 + price)  # Sell stock held by hold2
            hold2 = max(hold2, release1 - price)     # Buy stock after first sell
            
        return release2  # Maximum profit from two transactions
```

## Approach 2: Dynamic Programming with 2D DP Array

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def maxProfit(self, prices):
        if not prices or len(prices) <= 1:
            return 0

        k = 2  # Maximum number of transactions
        n = len(prices)
        # dp[i][j] = maximum profit using at most i transactions by time j
        dp = [[0] * n for _ in range(k + 1)]

        for i in range(1, k + 1):
            maxDiff = -prices[0]
            for j in range(1, n):
                # Previous maximum profit obtained with `i` transactions or profit after selling stock by day `j`
                dp[i][j] = max(dp[i][j - 1], prices[j] + maxDiff)
                # Max profit after buying stock on day `j`
                maxDiff = max(maxDiff, dp[i - 1][j] - prices[j])

        return dp[k][n - 1]  # Maximum profit with at most `k` transactions at the last day
```

## Approach 3: Forward and Backward Pass

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def maxProfit(self, prices):
        n = len(prices)
        if n <= 1:
            return 0

        leftProfits = [0] * n
        rightProfits = [0] * n

        minPrice = prices[0]
        for i in range(1, n):
            minPrice = min(minPrice, prices[i])
            leftProfits[i] = max(leftProfits[i - 1], prices[i] - minPrice)

        maxPrice = prices[n - 1]
        for i in range(n - 2, -1, -1):
            maxPrice = max(maxPrice, prices[i])
            rightProfits[i] = max(rightProfits[i + 1], maxPrice - prices[i])

        maxProfit = 0
        for i in range(n):
            maxProfit = max(maxProfit, leftProfits[i] + rightProfits[i])

        return maxProfit
```


