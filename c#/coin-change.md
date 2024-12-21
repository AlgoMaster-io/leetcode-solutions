# [Leetcode 322: Coin Change](https://leetcode.com/problems/coin-change/)

## Approaches
- [Brute Force Recursive Approach](#brute-force-recursive-approach)
- [Top-Down Dynamic Programming (Memoization)](#top-down-dynamic-programming-memoization)
- [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)

---

### Brute Force Recursive Approach

**Intuition:**

The problem can be approached from a recursive standpoint by considering the decision to include or exclude each coin from the total amount. For each coin, we attempt to create a solution by recursively calling the function and reducing the amount by the coin's value.

1. If the amount becomes exactly zero, we've found a valid way to distribute the coins, and we return 0 indicating 0 additional coins are needed.
2. If the amount becomes negative, it indicates this path won't yield a solution, so we return a large number (representing infinity).
3. We iterate over all coins and try each coin, keeping track of the minimum number of coins needed to make up the current amount.

**Code:**

```csharp
using System;

public class Solution {
    public int CoinChange(int[] coins, int amount) {
        // Recursive function to find the minimum coins needed
        return CoinChangeRecursive(coins, amount, new int[amount + 1]);
    }
    
    private int CoinChangeRecursive(int[] coins, int amount, int[] memo) {
        // If amount is negative, return infinity indicating no solution
        if (amount < 0) return int.MaxValue;

        // Base case: no coins needed for zero amount
        if (amount == 0) return 0;

        // Return the answer if it's precomputed
        if (memo[amount] != 0) return memo[amount];

        int min = int.MaxValue;

        foreach (int coin in coins) {
            int res = CoinChangeRecursive(coins, amount - coin, memo);

            if (res != int.MaxValue) {
                min = Math.Min(min, res + 1);
            }
        }

        memo[amount] = min;
        return min;
    }
}
```

**Time Complexity:** \(O(S^n)\) where \(S\) is the amount, and \(n\) is the number of coins.  
**Space Complexity:** \(O(S)\) for recursion stack and memoization.

---

### Top-Down Dynamic Programming (Memoization)

**Intuition:**

To optimize the recursive approach, we use memoization where we store the results of subproblems to avoid redundant calculations.

Instead of recalculating the minimum number of coins for a sub-amount multiple times, we check if we've already computed this value. If so, we use the precomputed value, otherwise, we compute it recursively and store it for future use.

**Code:**

```csharp
using System;

public class Solution {
    public int CoinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Array.Fill(dp, int.MaxValue);
        dp[0] = 0; // Base case

        for (int i = 1; i <= amount; i++) {
            foreach (int coin in coins) {
                if (i - coin >= 0 && dp[i - coin] != int.MaxValue) {
                    dp[i] = Math.Min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return dp[amount] == int.MaxValue ? -1 : dp[amount];
    }
}
```

**Time Complexity:** \(O(S \cdot n)\), where \(S\) is the amount and \(n\) is the number of coins.  
**Space Complexity:** \(O(S)\) for memoization array.

---

### Bottom-Up Dynamic Programming

**Intuition:**

This approach builds the solution iteratively starting from the smallest subproblems. We start with an amount of 0 and progressively compute the number of coins needed for amounts up to the given amount.

We use a DP array where each index represents the minimum number of coins needed for that amount. For each coin, if it can contribute to the sum, we update the DP array.

**Code:**

```csharp
using System;

public class Solution {
    public int CoinChange(int[] coins, int amount) {
        int max = amount + 1;
        int[] dp = new int[amount + 1];
        Array.Fill(dp, max);
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            foreach (int coin in coins) {
                if (i >= coin) {
                    dp[i] = Math.Min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

**Time Complexity:** \(O(S \cdot n)\), where \(S\) is the amount and \(n\) is the number of coins.  
**Space Complexity:** \(O(S)\) for the DP array.

This finishes our solution to the Coin Change problem using a variety of strategies starting with basic recursion and moving to iterative dynamic programming. Each approach has trade-offs in terms of time and space complexity, and DP provides the most optimal solution in terms of both.

