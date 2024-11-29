# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(S^n) where S is the amount and n is the number of coins
// Space Complexity: O(S) for the recursion stack
public class Solution {
    public int CoinChange(int[] coins, int amount) {
        if (amount == 0) return 0;
        int result = Helper(coins, amount);
        return result == int.MaxValue ? -1 : result;
    }

    private int Helper(int[] coins, int remainder) {
        if (remainder < 0) return int.MaxValue;
        if (remainder == 0) return 0;

        int minCoins = int.MaxValue;
        foreach (var coin in coins) {
            int res = Helper(coins, remainder - coin);
            
            // Avoid integer overflow here and take minimum
            if (res != int.MaxValue) {
                minCoins = Math.Min(minCoins, res + 1);
            }
        }
        return minCoins;
    }
}
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
csharp
```csharp
// Time Complexity: O(S * n)
// Space Complexity: O(S)
public class Solution {
    public int CoinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        // Initialize the dp array with a max value
        for (int i = 1; i <= amount; i++) {
            dp[i] = amount + 1;
        }
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            foreach (var coin in coins) {
                if (i - coin >= 0) {
                    dp[i] = Math.Min(dp[i], dp[i - coin] + 1);
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
csharp
```csharp
// Time Complexity: O(S * n)
// Space Complexity: O(S)
using System;

public class Solution {
    private int[] memo;

    public int CoinChange(int[] coins, int amount) {
        memo = new int[amount + 1];
        Array.Fill(memo, -1);
        memo[0] = 0;
        return CoinChangeRecursive(coins, amount);
    }

    private int CoinChangeRecursive(int[] coins, int remaining) {
        if (remaining < 0) return -1;
        if (memo[remaining] != -1) return memo[remaining];

        int minCoins = int.MaxValue;
        foreach (var coin in coins) {
            int res = CoinChangeRecursive(coins, remaining - coin);
            
            if (res >= 0) {
                minCoins = Math.Min(minCoins, res + 1);
            }
        }

        memo[remaining] = (minCoins == int.MaxValue) ? -1 : minCoins;
        return memo[remaining];
    }
}
```

