# [Leetcode 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approaches
- [Approach 1: Recursive Solution with Memoization](#approach-1)
- [Approach 2: Dynamic Programming (Forward Pass)](#approach-2)
- [Approach 3: Dynamic Programming with Space Optimization](#approach-3)

---

## Approach 1: Recursive Solution with Memoization

### Intuition
The idea is to create a recursive function that decides whether to buy, sell, or cooldown on a particular day. We'll utilize memoization to store results of subproblems (states) to avoid repetitive computations. The states can be defined as:
- Index `i` indicating the current day.
- Boolean `canBuy` indicating whether you can buy on the `i-th` day.

### Implementation
```typescript
function maxProfit(prices: number[]): number {
    // Creating a cache to store computed results for recursive states
    const memo = new Map<string, number>();

    function dfs(i: number, canBuy: boolean): number {
        // Base case: not array has reached end.
        if (i >= prices.length) return 0;
        
        // Check if this state has been calculated before.
        const key = `${i}-${canBuy}`;
        if (memo.has(key)) return memo.get(key)!;
        
        let profit: number;
        
        if (canBuy) {
            // Decision to buy or cooldown.
            let buy = dfs(i + 1, false) - prices[i];
            let cooldown = dfs(i + 1, true);
            profit = Math.max(buy, cooldown);
        } else {
            // Decision to sell or cooldown.
            let sell = dfs(i + 2, true) + prices[i]; // move 2 steps for cooldown
            let cooldown = dfs(i + 1, false);
            profit = Math.max(sell, cooldown);
        }
        
        // Store result in memoization table
        memo.set(key, profit);
        return profit;
    }

    // Start from day 0 with the possibility to buy
    return dfs(0, true);
}
```

### Complexity
- Time Complexity: O(n), where n is the number of days. Each state (`i, canBuy`) is computed once.
- Space Complexity: O(n) for recursive call stack and the memoization table.

---

## Approach 2: Dynamic Programming (Forward Pass)

### Intuition
This approach uses dynamic programming to create a table where each row is associated with a particular day and each column indicates whether we can buy, sell, or cooldown. We will iterate forward through the price list, updating our decisions:

- `dp[i][0]` maximum profit on the i-th day if we can buy.
- `dp[i][1]` maximum profit on the i-th day if we can sell.
- `dp[i][2]` maximum profit on the i-th day if we are in cooldown.

### Implementation
```typescript
function maxProfit(prices: number[]): number {
    const n = prices.length;
    if (n <= 1) return 0;

    // Initialize arrays for the three states on day 0.
    const dp = Array.from({ length: n }, () => [0, 0, 0]);

    // Base case for day 0
    dp[0][0] = 0; // cooldown
    dp[0][1] = -prices[0]; // buy
    dp[0][2] = 0; // sell

    for (let i = 1; i < n; i++) {
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][2]); // max between previous cooldown and sell
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]); // max between previous buy or buying today
        dp[i][2] = dp[i-1][1] + prices[i]; // sell today from previous buy
    }

    // Maximum profit will be max of cooldown or sell states on the last day
    return Math.max(dp[n-1][0], dp[n-1][2]);
}
```

### Complexity
- Time Complexity: O(n), iterating through prices once.
- Space Complexity: O(n) due to the dp table we maintain.

---

## Approach 3: Dynamic Programming with Space Optimization

### Intuition
In Approach 2, each state only relies on the previous day's states. Thus, instead of maintaining the complete dp array, we can use two variables to store results of the last day, reducing the space complexity.

### Implementation
```typescript
function maxProfit(prices: number[]): number {
    const n = prices.length;
    if (n <= 1) return 0;

    // Initialize previous states for the day 0.
    let prevCooldown = 0;
    let prevBuy = -prices[0];
    let prevSell = 0;

    for (let i = 1; i < n; i++) {
        let newCooldown = Math.max(prevCooldown, prevSell);
        let newBuy = Math.max(prevBuy, prevCooldown - prices[i]);
        let newSell = prevBuy + prices[i];

        prevCooldown = newCooldown;
        prevBuy = newBuy;
        prevSell = newSell;
    }

    // The maximum profit at the end would either be in the cooldown or sell states.
    return Math.max(prevCooldown, prevSell);
}
```

### Complexity
- Time Complexity: O(n), iterating through prices once.
- Space Complexity: O(1), using a constant amount of space for state variables.

