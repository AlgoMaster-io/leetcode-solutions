# 518. [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approach 1: Recursive Approach

### Solution
csharp
```csharp
// Time Complexity: O(2^n), where n is the number of coins
// Space Complexity: O(n)
public class Solution {
    public int Change(int amount, int[] coins) {
        return CountWays(coins, coins.Length, amount);
    }

    // Recursive function to count ways to make change
    private int CountWays(int[] coins, int numOfCoins, int amount) {
        // Base case: If amount is 0, there is 1 solution (to use no coins)
        if (amount == 0) return 1;
        
        // If amount is less than 0, there's no solution
        if (amount < 0) return 0;
        
        // If there are no coins and amount is more than 0, no solution
        if (numOfCoins <= 0 && amount > 0) return 0;

        // Count is the sum of solutions including coins[numOfCoins-1]
        // and excluding coins[numOfCoins-1]
        return CountWays(coins, numOfCoins - 1, amount) 
                + CountWays(coins, numOfCoins, amount - coins[numOfCoins - 1]);
    }
}
```

## Approach 2: Dynamic Programming (2D Array)

### Solution
csharp
```csharp
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(n * m)
public class Solution {
    public int Change(int amount, int[] coins) {
        // dp[i][j] means the number of ways to make change for amount j using first i types of coins
        int[,] dp = new int[coins.Length + 1, amount + 1];

        // Base case: If amount is 0, then there's exactly one way to make change: select no coin
        for (int i = 0; i <= coins.Length; i++) {
            dp[i, 0] = 1;
        }

        // Fill the dp array
        for (int i = 1; i <= coins.Length; i++) {
            for (int j = 1; j <= amount; j++) {
                // If we don't pick the coin, we have dp[i-1][j] possible ways
                dp[i, j] = dp[i - 1, j];

                // If we pick the coin, we add the ways we can make up the amount with this coin
                if (j >= coins[i - 1]) {
                    dp[i, j] += dp[i, j - coins[i - 1]];
                }
            }
        }

        return dp[coins.Length, amount];
    }
}
```

## Approach 3: Dynamic Programming (1D Array)

### Solution
csharp
```csharp
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(m)
public class Solution {
    public int Change(int amount, int[] coins) {
        // dp[j] means the number of ways to make change for amount j
        int[] dp = new int[amount + 1];
        dp[0] = 1; // Base case: one way to make zero amount

        // Traverse each coin
        foreach (int coin in coins) {
            // Update dp array by considering the current coin
            for (int j = coin; j <= amount; j++) {
                dp[j] += dp[j - coin];
            }
        }

        return dp[amount];
    }
}
```

