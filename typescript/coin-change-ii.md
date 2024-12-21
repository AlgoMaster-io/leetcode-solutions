# [Leetcode 518: Coin Change II](https://leetcode.com/problems/coin-change-2/)

## Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization-top-down-dynamic-programming)
- [Approach 3: Tabulation (Bottom-Up Dynamic Programming)](#approach-3-tabulation-bottom-up-dynamic-programming)
- [Approach 4: Space Optimized Dynamic Programming](#approach-4-space-optimized-dynamic-programming)

## Approach 1: Recursive Solution

### Intuition
The core idea is to use recursion to explore every possibility, where each coin can either be included or excluded from the combination. This solution directly translates the problem statement into code but is not efficient due to repeated calculations.

### TypeScript Code
```typescript
function change(amount: number, coins: number[]): number {
    // Recursive function definition
    function countCombinations(n: number, index: number): number {
        // Base cases
        // If amount becomes 0, one combination is found
        if (n === 0) return 1; 
        // If amount is negative or no coins left, no valid combination
        if (n < 0 || index === coins.length) return 0;

        // Choices: include the current coin or exclude it
        return countCombinations(n - coins[index], index) + countCombinations(n, index + 1);
    }

    // Initialize recursion
    return countCombinations(amount, 0);
}
```

### Time and Space Complexity
- **Time Complexity:** O(2^n) - Due to the exponential number of recursive calls.
- **Space Complexity:** O(n) - The recursive stack depth.

## Approach 2: Memoization (Top-Down Dynamic Programming)

### Intuition
By caching results of subproblems, we can prevent redundant calculations and significantly improve efficiency. We'll use a cache to store the number of ways to get each amount with the available coins up to a certain index.

### TypeScript Code
```typescript
function change(amount: number, coins: number[]): number {
    const memo: number[][] = Array.from({ length: coins.length + 1 }, () => Array(amount + 1).fill(-1));

    function countCombinations(n: number, index: number): number {
        if (n === 0) return 1;
        if (n < 0 || index === coins.length) return 0;

        // Use cached result if available
        if (memo[index][n] !== -1) return memo[index][n];

        // Store calculated result in memoization table
        memo[index][n] = countCombinations(n - coins[index], index) + countCombinations(n, index + 1);
        return memo[index][n];
    }

    return countCombinations(amount, 0);
}
```

### Time and Space Complexity
- **Time Complexity:** O(n * m) where n is the number of coins and m is the amount.
- **Space Complexity:** O(n * m) due to the memoization table.

## Approach 3: Tabulation (Bottom-Up Dynamic Programming)

### Intuition
This approach builds up a solution iteratively from smaller problems, filling a table where each entry `dp[i][j]` corresponds to the number of ways to make the amount `j` with the first `i` coins.

### TypeScript Code
```typescript
function change(amount: number, coins: number[]): number {
    const dp: number[][] = Array.from({ length: coins.length + 1 }, () => Array(amount + 1).fill(0));

    // Base case: There's one way to make the amount 0 (with no coins)
    for (let i = 0; i <= coins.length; i++) {
        dp[i][0] = 1;
    }

    for (let i = 1; i <= coins.length; i++) {
        for (let j = 0; j <= amount; j++) {
            // If using the ith coin
            if (j >= coins[i - 1]) {
                dp[i][j] = dp[i][j - coins[i - 1]] + dp[i - 1][j];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }

    return dp[coins.length][amount];
}
```

### Time and Space Complexity
- **Time Complexity:** O(n * m) where n is the number of coins and m is the amount.
- **Space Complexity:** O(n * m) for the dp table.

## Approach 4: Space Optimized Dynamic Programming

### Intuition
We can further optimize the space complexity by using a 1D array, as each entry only depends on the current and previous row in the dp table.

### TypeScript Code
```typescript
function change(amount: number, coins: number[]): number {
    const dp: number[] = Array(amount + 1).fill(0);
    dp[0] = 1;
    
    // For each coin, update the ways to make each amount
    for (const coin of coins) {
        for (let j = coin; j <= amount; j++) {
            dp[j] += dp[j - coin];
        }
    }
    
    return dp[amount];
}
```

### Time and Space Complexity
- **Time Complexity:** O(n * m) where n is the number of coins and m is the amount.
- **Space Complexity:** O(m) since we use a 1D dp array.

These solutions provide various approaches to tackling the Coin Change II problem, from the basic recursive approach to the more optimal space-efficient DP solution.

