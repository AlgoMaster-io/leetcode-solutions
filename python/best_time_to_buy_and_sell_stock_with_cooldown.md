# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def maxProfit(self, prices):
        n = len(prices)
        memo = [None] * n
        return self.calculate_max_profit(prices, 0, memo)
    
    def calculate_max_profit(self, prices, current_day, memo):
        if current_day >= len(prices):
            return 0
        if memo[current_day] is not None:
            return memo[current_day]

        max_profit = 0
        for sell_day in range(current_day + 1, len(prices)):
            # Calculate the profit for the current transaction
            profit = prices[sell_day] - prices[current_day] + self.calculate_max_profit(prices, sell_day + 2, memo)
            max_profit = max(max_profit, profit)

        memo[current_day] = max(max_profit, self.calculate_max_profit(prices, current_day + 1, memo))
        return memo[current_day]
```

## Approach 2: Dynamic Programming with State Variables

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def maxProfit(self, prices):
        if not prices:
            return 0
        n = len(prices)

        buy = [-prices[0]] * n
        sell = [0] * n
        cooldown = [0] * n

        for i in range(1, n):
            buy[i] = max(buy[i - 1], cooldown[i - 1] - prices[i])
            sell[i] = max(sell[i - 1], buy[i - 1] + prices[i])
            cooldown[i] = max(cooldown[i - 1], sell[i - 1])

        return max(sell[n - 1], cooldown[n - 1])
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxProfit(self, prices):
        if not prices:
            return 0

        buy = -prices[0]
        sell = 0
        cooldown = 0

        for i in range(1, len(prices)):
            new_sell = max(sell, buy + prices[i])
            new_buy = max(buy, cooldown - prices[i])
            cooldown = max(cooldown, sell)
            buy = new_buy
            sell = new_sell

        return max(sell, cooldown)
```

