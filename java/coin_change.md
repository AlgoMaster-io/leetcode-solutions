# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
```java
// Time Complexity: O(S^n) where S is the amount and n is the number of coins
// Space Complexity: O(S) for the recursion stack
public class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount == 0) return 0;
        int result = helper(coins, amount);
        return result == Integer.MAX_VALUE ? -1 : result;
    }

    private int helper(int[] coins, int remainder) {
        if (remainder < 0) return Integer.MAX_VALUE;
        if (remainder == 0) return 0;

        int minCoins = Integer.MAX_VALUE;
        for (int coin : coins) {
            int res = helper(coins, remainder - coin);
            
            // Avoid integer overflow here and take minimum
            if (res != Integer.MAX_VALUE) {
                minCoins = Math.min(minCoins, res + 1);
            }
        }
        return minCoins;
    }
}
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
```java
// Time Complexity: O(S * n)
// Space Complexity: O(S)
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        // Initialize the dp array with a max value
        for (int i = 1; i <= amount; i++) {
            dp[i] = amount + 1;
        }
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
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
```java
// Time Complexity: O(S * n)
// Space Complexity: O(S)
import java.util.Arrays;

public class Solution {
    private int[] memo;

    public int coinChange(int[] coins, int amount) {
        memo = new int[amount + 1];
        Arrays.fill(memo, -1);
        memo[0] = 0;
        return coinChangeRecursive(coins, amount);
    }

    private int coinChangeRecursive(int[] coins, int remaining) {
        if (remaining < 0) return -1;
        if (memo[remaining] != -1) return memo[remaining];

        int minCoins = Integer.MAX_VALUE;
        for (int coin : coins) {
            int res = coinChangeRecursive(coins, remaining - coin);
            
            if (res >= 0) {
                minCoins = Math.min(minCoins, res + 1);
            }
        }

        memo[remaining] = (minCoins == Integer.MAX_VALUE) ? -1 : minCoins;
        return memo[remaining];
    }
}
```


