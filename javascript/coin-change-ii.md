# [LeetCode 518: Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming - 2D Array](#dynamic-programming---2d-array)
3. [Dynamic Programming - 1D Array (Space Optimized)](#dynamic-programming---1d-array-space-optimized)

---

## Recursive Approach

### Intuition:
In this approach, we will try every combination of coins to make the amount. We explore each coin in two ways: include it or exclude it from the current coin set and proceed recursively for both choices. This brute-force approach ensures we explore all possibilities.

### Code:

```javascript
function change(amount, coins) {
    function solve(index, currentAmount) {
        // If we've reached the target amount
        if (currentAmount === amount) return 1;
        // If currentAmount overflows the target or no more coins left
        if (currentAmount > amount || index === coins.length) return 0;

        // Option 1: Include current coin, keep it in the next recursion
        const include = solve(index, currentAmount + coins[index]);

        // Option 2: Exclude current coin and move to the next coin
        const exclude = solve(index + 1, currentAmount);

        return include + exclude;
    }
    return solve(0, 0);
}
```

### Time and Space Complexity:

- **Time Complexity**: `O(2^n)` where `n` is the number of coins, as we explore possibilities for each coin with two choices.
- **Space Complexity**: `O(n)` considering the recursive stack depth.

---

## Dynamic Programming - 2D Array

### Intuition:
Instead of recalculating the results for the same state, we can use a 2D DP table `dp[i][j]` where `i` represents the number of coins considered, and `j` represents the target amount. `dp[i][j]` will store the number of ways to make the amount `j` using the first `i` coins.

### Code:

```javascript
function change(amount, coins) {
    const n = coins.length;
    const dp = Array.from({ length: n + 1 }, () => Array(amount + 1).fill(0));
    
    // Base case: there is one way to create the amount 0 - we don't include any coins.
    for (let i = 0; i <= n; i++) {
        dp[i][0] = 1;
    }

    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= amount; j++) {
            // If we don't take the coin
            dp[i][j] = dp[i - 1][j];

            // If we take the coin
            if (j >= coins[i - 1]) {
                dp[i][j] += dp[i][j - coins[i - 1]];
            }
        }
    }
    return dp[n][amount];
}
```

### Time and Space Complexity:

- **Time Complexity**: `O(n * amount)`, we calculate each subproblem once.
- **Space Complexity**: `O(n * amount)`, for storing results of each subproblem.

---

## Dynamic Programming - 1D Array (Space Optimized)

### Intuition:
The above 2D DP can be optimized to 1D. We only need results from the previous row, so we can overwrite the same dp array iteratively instead of maintaining a matrix.

### Code:

```javascript
function change(amount, coins) {
    const dp = Array(amount + 1).fill(0);
    dp[0] = 1; // Base case: there's one way to make up amount 0

    for (const coin of coins) {
        for (let j = coin; j <= amount; j++) {
            // Update the ways to generate amount `j` with current coin
            dp[j] += dp[j - coin];
        }
    }

    return dp[amount];
}
```

### Time and Space Complexity:

- **Time Complexity**: `O(n * amount)`, similar to the 2D approach.
- **Space Complexity**: `O(amount)`, reduced due to the single array usage. 

--- 

In all the approaches, the core is utilizing the coins in different ways to achieve the target with various computational efficiencies. The recursive solution is straightforward but impractical for large inputs. The dynamic programming solutions significantly improve both time and space complexity, with the 1D-space optimized solution being the most optimal in terms of space usage.

