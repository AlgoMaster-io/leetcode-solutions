# Leetcode Problem: [322. Coin Change](https://leetcode.com/problems/coin-change/)

## Solutions:
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming (Top-Down with Memoization)](#approach-2-dynamic-programming-top-down-with-memoization)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)

### Approach 1: Recursive Solution

**Intuition:**

The simplest way to solve the Coin Change problem is by using a recursive approach. For each coin, you have two choices: either take the coin or leave it. We recursively reduce the amount and count the minimum number of coins needed. This approach uses brute force and is highly inefficient due to recomputation and repeated exploration of the same states. 

**Time Complexity:**  
\( O(2^n) \) - We explore each possibility which results in a binary tree.

**Space Complexity:**  
\( O(n) \) - Maximum depth of recursion stack.

```typescript
function coinChange(coins: number[], amount: number): number {
    const recursive = (amount: number): number => {
        // Base case: If amount is zero, no coins are needed.
        if (amount === 0) return 0;
        // Base case: If amount is negative, it's impossible to give change.
        if (amount < 0) return -1;

        let minCoins = Infinity;

        for (const coin of coins) {
            // Explore using each coin
            const res = recursive(amount - coin);
            // If we can give change using this coin, update minCoins
            if (res >= 0) minCoins = Math.min(minCoins, 1 + res);
        }

        return minCoins === Infinity ? -1 : minCoins;
    }

    return recursive(amount);
}
```

---

### Approach 2: Dynamic Programming (Top-Down with Memoization)

**Intuition:**

To optimize the recursive approach, we can use a top-down dynamic programming with memoization. We store results of subproblems so we don't recompute results unnecessarily, which allows us to solve the problem efficiently.

**Time Complexity:**  
\( O(n \times \text{amount}) \) - n is the number of coins, and we solve each subproblem once.

**Space Complexity:**  
\( O(\text{amount}) \) - For the recursion stack and memoization.

```typescript
function coinChange(coins: number[], amount: number): number {
    const memo = new Map<number, number>();

    const dp = (amount: number): number => {
        // Base case: If amount is zero, no coins are needed.
        if (amount === 0) return 0;
        // If amount is negative, return -1 indicating not possible.
        if (amount < 0) return -1;

        // Return the result if it's already in the memo.
        if (memo.has(amount)) return memo.get(amount)!;

        let minCoins = Infinity;

        for (const coin of coins) {
            const res = dp(amount - coin);
            if (res >= 0) minCoins = Math.min(minCoins, 1 + res);
        }

        // Memoize the result.
        memo.set(amount, minCoins === Infinity ? -1 : minCoins);

        return memo.get(amount)!;
    }

    return dp(amount);
}
```

---

### Approach 3: Dynamic Programming (Bottom-Up)

**Intuition:**

We can solve this problem iteratively using a bottom-up approach. We maintain an array `dp` where `dp[i]` is the minimum number of coins needed to make up amount `i`. For each amount `i`, iterate over each coin and update the `dp[i]` using previously computed results.

**Time Complexity:**  
\( O(n \times \text{amount}) \) - For each amount, we iterate through all coins.

**Space Complexity:**  
\( O(\text{amount}) \) - The space required for the dp array.

```typescript
function coinChange(coins: number[], amount: number): number {
    const dp = new Array(amount + 1).fill(amount + 1);
    dp[0] = 0;

    // Build up the dp table
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            // If i - coin is valid, consider this coin
            if (i - coin >= 0) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
    }

    return dp[amount] === amount + 1 ? -1 : dp[amount];
}
```

All solutions aim to find the minimum number of coins that make up a given amount, but the final DP approach offers the most efficient solution by systematically building a solution that minimizes recomputation.

