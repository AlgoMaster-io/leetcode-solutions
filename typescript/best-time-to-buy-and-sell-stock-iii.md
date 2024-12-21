# [Problem 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming - 3D DP Array](#approach-2-dynamic-programming-3d-dp-array)
- [Approach 3: Dynamic Programming - Optimized Space](#approach-3-dynamic-programming-optimized-space)

## Approach 1: Brute Force

### Intuition
The brute force solution involves trying every pair of buy and sell days for two transactions and calculating the resulting profits. This approach would involve computing the profit for all possible combinations of two transactions which requires a nested iteration over the stock prices.

### Code

```typescript
function maxProfit(prices: number[]): number {
    const n = prices.length;
    let maxProfit = 0;
    
    // Go through all combinations of two transactions
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Calculate profit for the first transaction
            let firstProfit = Math.max(0, prices[j] - prices[i]);
            // Now consider the second transaction
            for (let k = j + 1; k < n; k++) {
                for (let l = k + 1; l < n; l++) {
                    let secondProfit = Math.max(0, prices[l] - prices[k]);
                    // Sum up the profits
                    maxProfit = Math.max(maxProfit, firstProfit + secondProfit);
                }
            }
        }
    }
    
    return maxProfit;
}
```

### Complexity
- **Time Complexity**: O(n^4) - Due to four nested loops.
- **Space Complexity**: O(1) - Only a constant amount of space used.

## Approach 2: Dynamic Programming - 3D DP Array

### Intuition
Dynamic programming offers a way to efficiently solve this problem by keeping track of the maximum profit up to each day for up to two transactions. We use a 3D array `dp` where `dp[i][j][k]` represents the maximum profit on the `i-th` day with `j` transactions and holding (`k=1`) or not holding (`k=0`) a stock.

### Code

```typescript
function maxProfit(prices: number[]): number {
    const n = prices.length;
    if (n == 0) return 0;
    
    const dp = new Array(n).fill(0).map(() => new Array(3).fill(0).map(() => new Array(2).fill(0)));
    
    // Initialization
    for (let i = 0; i < n; i++) {
        dp[i][0][1] = -Infinity; // Impossible to hold stock without any transactions
        for (let j = 0; j <= 2; j++) {
            dp[i][j][0] = 0; // No profit without any stocks held
        }
    }
    
    for (let j = 0; j <= 2; j++) {
        dp[0][j][1] = -prices[0];
    }
    
    for (let i = 1; i < n; i++) {
        for (let j = 1; j <= 2; j++) {
            dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
            dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
        }
    }
    
    return Math.max(dp[n - 1][1][0], dp[n - 1][2][0]);
}
```

### Complexity
- **Time Complexity**: O(n \* k) where k is the number of transactions (in this case 2), effectively O(n).
- **Space Complexity**: O(n \* k \* 2) = O(n) since k is constant.

## Approach 3: Dynamic Programming - Optimized Space

### Intuition
We can optimize the space used in the dynamic programming approach by keeping only the previous day's state instead of storing the entire DP table. This reduces the space complexity to a constant factor.

### Code

```typescript
function maxProfit(prices: number[]): number {
    const n = prices.length;
    if (n == 0) return 0;

    let dp0 = 0, dp1 = -Infinity; // No transactions yet
    let dp2 = 0, dp3 = -Infinity; // One transaction done

    for (let i = 0; i < n; i++) {
        dp0 = Math.max(dp0, dp1 + prices[i]); // Decide not to hold
        dp1 = Math.max(dp1, -prices[i]); // Decide to buy
        dp2 = Math.max(dp2, dp3 + prices[i]); // Finish second transaction
        dp3 = Math.max(dp3, dp0 - prices[i]); // Start second transaction
    }

    return dp2; // Maximum profit after finishing two transactions
}
```

### Complexity
- **Time Complexity**: O(n) - Single pass through prices array.
- **Space Complexity**: O(1) - Constant space used. 

These solutions progress from the brute force method to efficient dynamic programming, offering a comprehensive guide to solving the problem by structuring the state transition carefully.

