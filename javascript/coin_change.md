# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
```javascript
// Time Complexity: O(S^n) where S is the amount and n is the number of coins
// Space Complexity: O(S) for the recursion stack
class Solution {
    coinChange(coins, amount) {
        if (amount === 0) return 0;
        const result = this.helper(coins, amount);
        return result === Number.MAX_SAFE_INTEGER ? -1 : result;
    }

    helper(coins, remainder) {
        if (remainder < 0) return Number.MAX_SAFE_INTEGER;
        if (remainder === 0) return 0;

        let minCoins = Number.MAX_SAFE_INTEGER;
        for (const coin of coins) {
            const res = this.helper(coins, remainder - coin);
            
            // Avoid integer overflow here and take minimum
            if (res !== Number.MAX_SAFE_INTEGER) {
                minCoins = Math.min(minCoins, res + 1);
            }
        }
        return minCoins;
    }
}
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
```javascript
// Time Complexity: O(S * n)
// Space Complexity: O(S)
class Solution {
    coinChange(coins, amount) {
        const dp = new Array(amount + 1).fill(amount + 1);
        dp[0] = 0;

        for (let i = 1; i <= amount; i++) {
            for (const coin of coins) {
                if (i - coin >= 0) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        // If dp[amount] is still amount + 1, no solution found
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

## Approach 3: Dynamic Programming - Top Down with Memoization

### Solution
```javascript
// Time Complexity: O(S * n)
// Space Complexity: O(S)
class Solution {
    constructor() {
        this.memo = [];
    }

    coinChange(coins, amount) {
        this.memo = new Array(amount + 1).fill(-1);
        this.memo[0] = 0;
        return this.coinChangeRecursive(coins, amount);
    }

    coinChangeRecursive(coins, remaining) {
        if (remaining < 0) return -1;
        if (this.memo[remaining] !== -1) return this.memo[remaining];

        let minCoins = Number.MAX_SAFE_INTEGER;
        for (const coin of coins) {
            const res = this.coinChangeRecursive(coins, remaining - coin);
            
            if (res >= 0) {
                minCoins = Math.min(minCoins, res + 1);
            }
        }

        this.memo[remaining] = (minCoins === Number.MAX_SAFE_INTEGER) ? -1 : minCoins;
        return this.memo[remaining];
    }
}
```

