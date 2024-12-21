[Leetcode Problem 518: Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

### Approach 1: Recursion

**Intuition**:
- The problem can be solved using recursive methods where we make a choice at each step: either include the current coin or exclude it.
- We aim to calculate the total number of combinations that make up the amount using available coins.

**Detailed Steps**:
1. Start from the first coin and decide for each coin whether to include it in the current combination or not.
2. If included, reduce the target amount by coin's value and continue with the same coin.
3. If not included, move to the next coin.
4. Base cases:
   - If `amount` becomes 0, it means we have found a valid combination.
   - If `amount` becomes negative or we run out of coins, it means the current path isn't valid.

```csharp
public class Solution {
    public int Change(int amount, int[] coins) {
        return CountCombinations(coins, amount, 0);
    }
    
    private int CountCombinations(int[] coins, int remainingAmount, int currentIndex) {
        // Base case: If exact amount is achieved, we found one combination
        if (remainingAmount == 0) return 1;
        
        // Base case: If out of coins or amount less than 0, no valid combination
        if (remainingAmount < 0 || currentIndex >= coins.Length) return 0;
        
        // Include current coin and continue with the same index, or skip coin and move forward
        int withCoin = CountCombinations(coins, remainingAmount - coins[currentIndex], currentIndex);
        int withoutCoin = CountCombinations(coins, remainingAmount, currentIndex + 1);
        
        return withCoin + withoutCoin;
    }
}
```

- **Time Complexity**: O(2^n) where `n` is the number of coins.
- **Space Complexity**: O(n) due to recursion stack.

### Approach 2: Recursion with Memoization

**Intuition**:
- Many subproblems overlap. By storing the results of subproblems, we can avoid redundant calculations.

**Detailed Steps**:
1. Use a 2-D memo array where `memo[currentIndex][remainingAmount]` stores the result for given index and amount.
2. Check the memo table before calculating the same subproblem.
3. Use the recursive logic from Approach 1 and save the results in the memoization table.

```csharp
public class Solution {
    public int Change(int amount, int[] coins) {
        int[,] memo = new int[coins.Length, amount + 1];
        return CountCombinations(coins, amount, 0, memo);
    }
    
    private int CountCombinations(int[] coins, int remainingAmount, int currentIndex, int[,] memo) {
        if (remainingAmount == 0) return 1;
        if (remainingAmount < 0 || currentIndex >= coins.Length) return 0;
        
        if (memo[currentIndex, remainingAmount] != 0) return memo[currentIndex, remainingAmount];
        
        int withCoin = CountCombinations(coins, remainingAmount - coins[currentIndex], currentIndex, memo);
        int withoutCoin = CountCombinations(coins, remainingAmount, currentIndex + 1, memo);
        
        memo[currentIndex, remainingAmount] = withCoin + withoutCoin;
        return memo[currentIndex, remainingAmount];
    }
}
```

- **Time Complexity**: O(n * m) where `n` is the number of coins and `m` is the amount.
- **Space Complexity**: O(n * m) for memoization table and stack space.

### Approach 3: Dynamic Programming

**Intuition**:
- Convert the recursive solution to iterative using a dynamic programming (DP) array.
- dp[i] represents the number of ways to make amount `i` using the coins.

**Detailed Steps**:
1. Initialize a DP array of size `amount + 1` and set `dp[0]` to 1 (no coins needed to make amount zero).
2. For each coin, update the DP array from the current coin's value up to the amount.
3. For each amount, add the number of ways of making the amount without the current coin.

```csharp
public class Solution {
    public int Change(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;  // Base case: One way to get amount 0, use zero coins
        
        foreach (var coin in coins) {
            for (int j = coin; j <= amount; j++) {
                dp[j] += dp[j - coin];  // Add ways using the current coin
            }
        }
        
        return dp[amount];
    }
}
```

- **Time Complexity**: O(n * m) where `n` is the number of coins and `m` is the amount.
- **Space Complexity**: O(m) as we use a single array.

