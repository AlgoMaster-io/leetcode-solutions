# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function maxProfit(prices: number[]): number {
    const n: number = prices.length;
    const memo: (number | null)[] = new Array(n).fill(null);
    return calculateMaxProfit(prices, 0, memo);
}

function calculateMaxProfit(prices: number[], currentDay: number, memo: (number | null)[]): number {
    if (currentDay >= prices.length) {
        return 0;
    }
    if (memo[currentDay] !== null) {
        return memo[currentDay];
    }

    let maxProfit: number = 0;
    for (let sellDay = currentDay + 1; sellDay < prices.length; sellDay++) {
        // Calculate the profit for the current transaction
        const profit: number = prices[sellDay] - prices[currentDay] + calculateMaxProfit(prices, sellDay + 2, memo);
        maxProfit = Math.max(maxProfit, profit);
    }

    memo[currentDay] = Math.max(maxProfit, calculateMaxProfit(prices, currentDay + 1, memo));
    return memo[currentDay];
}
```

## Approach 2: Dynamic Programming with State Variables

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function maxProfit(prices: number[]): number {
    if (prices.length === 0) {
        return 0;
    }
    const n: number = prices.length;

    const buy: number[] = new Array(n).fill(0);
    const sell: number[] = new Array(n).fill(0);
    const cooldown: number[] = new Array(n).fill(0);

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
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function maxProfit(prices: number[]): number {
    if (prices.length === 0) {
        return 0;
    }

    let buy: number = -prices[0];
    let sell: number = 0;
    let cooldown: number = 0;

    for (let i = 1; i < prices.length; i++) {
        const newSell: number = Math.max(sell, buy + prices[i]);
        const newBuy: number = Math.max(buy, cooldown - prices[i]);
        cooldown = Math.max(cooldown, sell);
        buy = newBuy;
        sell = newSell;
    }

    return Math.max(sell, cooldown);
}
```

