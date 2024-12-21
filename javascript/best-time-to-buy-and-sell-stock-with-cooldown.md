# [Leetcode 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Dynamic Programming with Space Optimization](#approach-3-dynamic-programming-with-space-optimization)

## Problem Description

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i`th day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

## Approach 1: Recursive Solution

### Intuition
We can solve this problem using a simple recursive approach by considering every decision:
1. We buy the stock on day `i`, which means we will have a pending transaction.
2. We sell the stock on day `j` (j > i) and then add the cooldown day.
3. We skip the day.

### JavaScript Code

```javascript
const maxProfitRecursive = (prices) => {
    const calculateProfit = (i, canBuy) => {
        if (i >= prices.length) return 0;
        
        let cooldown = calculateProfit(i + 1, canBuy);
        if (canBuy) {
            // We have the option to buy stock
            let buy = -prices[i] + calculateProfit(i + 1, false);
            return Math.max(buy, cooldown);
        } else {
            // We have the option to sell the stock
            let sell = prices[i] + calculateProfit(i + 2, true);
            return Math.max(sell, cooldown);
        }
    };
    
    return calculateProfit(0, true);
};

// Example usage:
console.log(maxProfitRecursive([1, 2, 3, 0, 2])); // Output: 3
```

### Complexity Analysis
- **Time Complexity**: O(2^n), where n is the number of days. This is because we explore two paths (buy or cooldown or sell) for each day.
- **Space Complexity**: O(n), due to the recursion stack.

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition
To avoid recalculating results for the same state, we can use memoization to store already computed results. This will reduce the time complexity as we won't compute the same state more than once.

### JavaScript Code

```javascript
const maxProfitMemoization = (prices) => {
    const memo = new Array(prices.length).fill(null).map(() => new Array(2).fill(undefined));
    
    const calculateProfit = (i, canBuy) => {
        if (i >= prices.length) return 0;
        if (memo[i][canBuy] !== undefined) return memo[i][canBuy];
        
        let cooldown = calculateProfit(i + 1, canBuy);
        if (canBuy) {
            let buy = -prices[i] + calculateProfit(i + 1, false);
            memo[i][canBuy] = Math.max(buy, cooldown);
        } else {
            let sell = prices[i] + calculateProfit(i + 2, true);
            memo[i][canBuy] = Math.max(sell, cooldown);
        }
        
        return memo[i][canBuy];
    };
    
    return calculateProfit(0, true);
};

// Example usage:
console.log(maxProfitMemoization([1, 2, 3, 0, 2])); // Output: 3
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of days, as each state is computed only once.
- **Space Complexity**: O(n), due to the `memo` array and the recursion stack.

## Approach 3: Dynamic Programming with Space Optimization

### Intuition
We can further optimize our solution using the bottom-up DP approach with space optimization. We only need the last two states to calculate the current state, reducing space complexity significantly.

### JavaScript Code

```javascript
const maxProfitDP = (prices) => {
    if (prices.length === 0) return 0;
    
    let n = prices.length;
    let sell = 0, prev_sell = 0, buy = -prices[0];
    
    for (let i = 1; i < n; i++) {
        let tmp = sell;
        sell = Math.max(sell, buy + prices[i]);
        buy = Math.max(buy, prev_sell - prices[i]);
        prev_sell = tmp;
    }
    
    return sell;
};

// Example usage:
console.log(maxProfitDP([1, 2, 3, 0, 2])); // Output: 3
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of days.
- **Space Complexity**: O(1), as we are using a constant amount of extra space.

By exploring recursive, memoized, and dynamic programming solutions with space optimization, we have provided a comprehensive understanding of how to tackle the problem "Best Time to Buy and Sell Stock with Cooldown". Each step optimizes on the last, improving both time and space complexities for a practical implementation.

