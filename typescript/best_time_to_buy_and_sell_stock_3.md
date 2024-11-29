# 123. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approach 1: Dynamic Programming with State Variables

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    public maxProfit(prices: number[]): number {
        let hold1 = Number.MIN_SAFE_INTEGER, hold2 = Number.MIN_SAFE_INTEGER;
        let release1 = 0, release2 = 0;

        for (const price of prices) {
            // First transaction
            release1 = Math.max(release1, hold1 + price);  // Sell stock held by hold1
            hold1 = Math.max(hold1, -price);               // Buy stock

            // Second transaction
            release2 = Math.max(release2, hold2 + price);  // Sell stock held by hold2
            hold2 = Math.max(hold2, release1 - price);     // Buy stock after first sell
        }
        
        return release2; // Maximum profit from two transactions
    }
}
```

## Approach 2: Dynamic Programming with 2D DP Array

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    public maxProfit(prices: number[]): number {
        if (prices === null || prices.length <= 1) return 0;

        const k = 2; // Maximum number of transactions
        const n = prices.length;
        // dp[i][j] = maximum profit using at most i transactions by time j
        const dp: number[][] = Array.from({ length: k + 1 }, () => Array(n).fill(0));

        for (let i = 1; i <= k; i++) {
            let maxDiff = -prices[0];
            for (let j = 1; j < n; j++) {
                // Previous maximum profit obtained with `i` transactions or profit after selling stock by day `j`
                dp[i][j] = Math.max(dp[i][j - 1], prices[j] + maxDiff);
                // Max profit after buying stock on day `j`
                maxDiff = Math.max(maxDiff, dp[i - 1][j] - prices[j]);
            }
        }
        return dp[k][n - 1]; // Maximum profit with at most `k` transactions at the last day
    }
}
```

## Approach 3: Forward and Backward Pass

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    public maxProfit(prices: number[]): number {
        const n = prices.length;
        if (n <= 1) return 0;

        const leftProfits: number[] = new Array(n).fill(0);
        const rightProfits: number[] = new Array(n).fill(0);

        let minPrice = prices[0];
        for (let i = 1; i < n; i++) {
            minPrice = Math.min(minPrice, prices[i]);
            leftProfits[i] = Math.max(leftProfits[i - 1], prices[i] - minPrice);
        }

        let maxPrice = prices[n - 1];
        for (let i = n - 2; i >= 0; i--) {
            maxPrice = Math.max(maxPrice, prices[i]);
            rightProfits[i] = Math.max(rightProfits[i + 1], maxPrice - prices[i]);
        }

        let maxProfit = 0;
        for (let i = 0; i < n; i++) {
            maxProfit = Math.max(maxProfit, leftProfits[i] + rightProfits[i]);
        }

        return maxProfit;
    }
}
```

