# [Leetcode 322: Coin Change](https://leetcode.com/problems/coin-change/)

## Approaches
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)

### Approach 1: Recursion with Memoization

#### Intuition
The core idea is to try every combination of coins to find the minimum number of coins needed to form the target amount. This can be efficiently achieved by using a recursive function to explore all possibilities while utilizing memoization to store and reuse the results of previously computed subproblems to avoid redundant calculations.

#### Steps
1. **Recursive Function**: Define a recursive function that aims to compute the minimum coins needed for a given `amount`.
2. **Base Case**: If `amount` is 0, return 0 because no coins are needed.
3. **Iterate Over Coins**: For each coin, compute the number of coins needed for the subproblem `amount - coin`.
4. **Store Results**: Use a memoization array to store the results of subproblems.
5. **Return Result**: If a subproblem leads to a positive result, update the minimum coins needed. Otherwise, return -1 if the target cannot be met.

#### Code
```javascript
var coinChange = function(coins, amount) {
    // Memoization to store results of subproblems
    const memo = new Array(amount + 1).fill(null);

    // Helper function to find the minimum coins needed
    const helper = (remaining) => {
        // Base case: if no amount is left, no coins are needed
        if (remaining === 0) return 0;
        // If the amount is negative, it's not possible to make the amount
        if (remaining < 0) return -1;
        // Return already computed results
        if (memo[remaining] !== null) return memo[remaining];
        
        let minCoins = Infinity;
        
        // Try using each coin
        for (let coin of coins) {
            const res = helper(remaining - coin);
            // If the subproblem returned a valid result, update minCoins
            if (res >= 0) {
                minCoins = Math.min(minCoins, res + 1);
            }
        }
        // If no valid combination found, store -1
        memo[remaining] = (minCoins === Infinity) ? -1 : minCoins;
        return memo[remaining];
    };

    return helper(amount);
};
```

#### Complexity
- **Time Complexity**: O(S * n), where S is the amount and n is the number of coins.
- **Space Complexity**: O(S), for the recursion stack and memoization array.

---

### Approach 2: Dynamic Programming (Bottom-Up)

#### Intuition
The problem can be approached as a dynamic programming problem where we build up the solution by solving subproblems from the ground up. We use a table `dp[]` where `dp[i]` represents the minimum number of coins required to make the amount `i`.

#### Steps
1. **DP Array Initialization**: Initialize an array `dp` of size `amount + 1` with `Infinity` (a large number), except `dp[0] = 0` (no coins needed for amount `0`).
2. **Iterate Through Amounts**: For each amount from 1 to `amount`, determine the minimum coins required.
3. **Iterate Through Coins**: For each coin, update the dp value for the current amount if using the coin gives a better result.
4. **Return Result**: The answer is stored in `dp[amount]`. If it is still `Infinity`, return -1.

#### Code
```javascript
var coinChange = function(coins, amount) {
    // Initialize dp array
    const dp = new Array(amount + 1).fill(Infinity);
    dp[0] = 0; // no coins are needed for amount 0

    // Build up the dp table
    for (let i = 1; i <= amount; i++) {
        for (let coin of coins) {
            if (i - coin >= 0) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }

    // If dp[amount] is still Infinity, it means we can't form that amount
    return dp[amount] === Infinity ? -1 : dp[amount];
};
```

#### Complexity
- **Time Complexity**: O(S * n), where S is the amount and n is the number of coins.
- **Space Complexity**: O(S), where S is the amount.

In summary, both approaches provide viable solutions; however, the dynamic programming approach is generally more straightforward and typically more efficient with less overhead than recursion with memoization.

