## [Leetcode Problem 322: Coin Change](https://leetcode.com/problems/coin-change/)

### Approaches:
1. [Brute Force - Recursive](#approach-1-brute-force-recursive)
2. [Dynamic Programming - Top Down (Memoization)](#approach-2-dynamic-programming-top-down-memoization)
3. [Dynamic Programming - Bottom Up (Tabulation)](#approach-3-dynamic-programming-bottom-up-tabulation)

### Approach 1: Brute Force - Recursive

The brute force approach is to try all possible combinations of coins to determine the minimum number needed for the given amount. The intuition is to recursively subtract each coin from the amount and solve the problem for the reduced amount until the amount is 0. This tries out all possible combinations.

```java
public class CoinChange {

    public int coinChange(int[] coins, int amount) {
        if (amount < 0) return -1;   // Base case: if amount is negative, no solution
        if (amount == 0) return 0;   // Base case: zero coins needed for zero amount

        int minCoins = Integer.MAX_VALUE;
        for (int coin : coins) {
            int result = coinChange(coins, amount - coin); // Reduce the amount by the coin value
            if (result >= 0 && result < minCoins) { // Valid result
                minCoins = result + 1; // Counting the current coin
            }
        }
        return (minCoins == Integer.MAX_VALUE) ? -1 : minCoins; // Check if found a valid solution
    }
}
```

- **Time Complexity:** O(S^n) where S is the amount and n is the number of coins, as we explore all possibilities.
- **Space Complexity:** O(S) due to the recursion stack.

### Approach 2: Dynamic Programming - Top Down (Memoization)

To optimize the recursive solution, we use memoization to store the results of subproblems to avoid redundant calculations. The idea is the same as the recursive approach but with a cache to store previously computed results.

```java
import java.util.HashMap;
import java.util.Map;

public class CoinChange {

    public int coinChange(int[] coins, int amount) {
        Map<Integer, Integer> memo = new HashMap<>(); // Memoization table
        return coinChangeHelper(coins, amount, memo);
    }

    private int coinChangeHelper(int[] coins, int rem, Map<Integer, Integer> memo) {
        if (rem < 0) return -1;
        if (rem == 0) return 0;
        if (memo.containsKey(rem)) return memo.get(rem);

        int minCoins = Integer.MAX_VALUE;
        for (int coin : coins) {
            int result = coinChangeHelper(coins, rem - coin, memo);
            if (result >= 0 && result < minCoins) {
                minCoins = result + 1;
            }
        }
        memo.put(rem, (minCoins == Integer.MAX_VALUE) ? -1 : minCoins);
        return memo.get(rem);
    }
}
```

- **Time Complexity:** O(S * n) where S is the amount and n is the number of coins.
- **Space Complexity:** O(S) for the memoization table.

### Approach 3: Dynamic Programming - Bottom Up (Tabulation)

The most efficient approach uses dynamic programming with a bottom-up technique by creating a table to store the minimum coins required for all values from 0 to the amount. We build the solution iteratively.

```java
public class CoinChange {

    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1]; 
        // Initialize dp array, amount + 1 is used as Integer.MAX_VALUE causing overflow
        Arrays.fill(dp, amount + 1);
        dp[0] = 0; // Base case: zero coins needed for zero amount
        
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) { // If the coin value is less than the amount needed
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1); // Recurrence relation
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

- **Time Complexity:** O(S * n) where S is the amount and n is the number of coins.
- **Space Complexity:** O(S) for the DP table.

Each approach builds on optimizations of previous methods, providing a comprehensive view of the problem-solving process. Choose the most optimal one, the bottom-up dynamic programming approach, for production implementations due to its efficiency in both time and space.

