# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    maxProfit(prices) {
        const n = prices.length;
        const memo = new Array(n);
        return this.calculateMaxProfit(prices, 0, memo);
    }

    calculateMaxProfit(prices, currentDay, memo) {
        if (currentDay >= prices.length) {
            return 0;
        }
        if (memo[currentDay] !== undefined) {
            return memo[currentDay];
        }

        let maxProfit = 0;
        for (let sellDay = currentDay + 1; sellDay < prices.length; sellDay++) {
            // Calculate the profit for the current transaction
            const profit = prices[sellDay] - prices[currentDay] + this.calculateMaxProfit(prices, sellDay + 2, memo);
            maxProfit = Math.max(maxProfit, profit);
        }

        memo[currentDay] = Math.max(maxProfit, this.calculateMaxProfit(prices, currentDay + 1, memo));
        return memo[currentDay];
    }
}
```

## Approach 2: Dynamic Programming with State Variables

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    maxProfit(prices) {
        if (prices.length === 0) {
            return 0;
        }
        const n = prices.length;

        const buy = new Array(n).fill(0);
        const sell = new Array(n).fill(0);
        const cooldown = new Array(n).fill(0);

        buy[0] = -prices[0];
        sell[0] = 0;
        cooldown[0] = 0;

        for (let i = 1; i < n; i++) {
            buy[i] = Math.max(buy[i - 1], cooldown[i - 1] - prices[i]);
            sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);
            cooldown[i] = Math.max(cooldown[i - 1], sell[i - 1]);
        }

        return Math.max(sell[n - 1], cooldown[n - 1]);
    }
}
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    maxProfit(prices) {
        if (prices.length === 0) {
            return 0;
        }

        let buy = -prices[0];
        let sell = 0;
        let cooldown = 0;

        for (let i = 1; i < prices.length; i++) {
            let newSell = Math.max(sell, buy + prices[i]);
            let newBuy = Math.max(buy, cooldown - prices[i]);
            cooldown = Math.max(cooldown, sell);
            buy = newBuy;
            sell = newSell;
        }

        return Math.max(sell, cooldown);
    }
}
```

