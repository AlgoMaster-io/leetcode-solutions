# 518. [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approach 1: Recursive Approach

### Solution
typescript
```typescript
// Time Complexity: O(2^n), where n is the number of coins
// Space Complexity: O(n)
class Solution {
    change(amount: number, coins: number[]): number {
        return this.countWays(coins, coins.length, amount);
    }

    // Recursive function to count ways to make change
    private countWays(coins: number[], numOfCoins: number, amount: number): number {
        // Base case: If amount is 0, there is 1 solution (to use no coins)
        if (amount === 0) return 1;

        // If amount is less than 0, there's no solution
        if (amount < 0) return 0;

        // If there are no coins and amount is more than 0, no solution
        if (numOfCoins <= 0 && amount > 0) return 0;

        // Count is the sum of solutions including coins[numOfCoins-1]
        // and excluding coins[numOfCoins-1]
        return this.countWays(coins, numOfCoins - 1, amount) 
                + this.countWays(coins, numOfCoins, amount - coins[numOfCoins - 1]);
    }
}
```

## Approach 2: Dynamic Programming (2D Array)

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(n * m)
class Solution {
    change(amount: number, coins: number[]): number {
        // dp[i][j] means the number of ways to make change for amount j using first i types of coins
        const dp = Array.from({ length: coins.length + 1 }, () => Array(amount + 1).fill(0));

        // Base case: If amount is 0, then there's exactly one way to make change: select no coin
        for (let i = 0; i <= coins.length; i++) {
            dp[i][0] = 1;
        }

        // Fill the dp array
        for (let i = 1; i <= coins.length; i++) {
            for (let j = 1; j <= amount; j++) {
                // If we don't pick the coin, we have dp[i-1][j] possible ways
                dp[i][j] = dp[i - 1][j];

                // If we pick the coin, we add the ways we can make up the amount with this coin
                if (j >= coins[i - 1]) {
                    dp[i][j] += dp[i][j - coins[i - 1]];
                }
            }
        }

        return dp[coins.length][amount];
    }
}
```

## Approach 3: Dynamic Programming (1D Array)

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(m)
class Solution {
    change(amount: number, coins: number[]): number {
        // dp[j] means the number of ways to make change for amount j
        const dp = Array(amount + 1).fill(0);
        dp[0] = 1; // Base case: one way to make zero amount

        // Traverse each coin
        for (const coin of coins) {
            // Update dp array by considering the current coin
            for (let j = coin; j <= amount; j++) {
                dp[j] += dp[j - coin];
            }
        }

        return dp[amount];
    }
}
```

