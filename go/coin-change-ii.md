# [Leetcode 518: Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming - 2D Array](#dynamic-programming---2d-array)
3. [Dynamic Programming - 1D Array](#dynamic-programming---1d-array)

---

### Recursive Approach

#### Intuition
The basic idea of this approach is to use recursion to explore all possible combinations of coins that sum up to the target amount. For each coin, we have the choice of including it in our combination or not. This is a typical recursion problem, where at each step, we consider including the current coin in our solution or skipping it.

#### Code

```go
func change(amount int, coins []int) int {
    // Initialize the recursive function starting with first coin and the target amount
    return helper(coins, amount, 0)
}

func helper(coins []int, amount int, index int) int {
    // If amount becomes zero, we found a way to make the change
    if amount == 0 {
        return 1
    }
    // If amount becomes negative or we have considered all coins
    if amount < 0 || index == len(coins) {
        return 0
    }
    // Choose the current coin and recursively try to find other combinations
    chooseCurrent = helper(coins, amount-coins[index], index)
    // Skip the current coin and try others
    skipCurrent = helper(coins, amount, index+1)
    
    // Total ways by choosing or skipping the current coin
    return chooseCurrent + skipCurrent
}
```

#### Time and Space Complexity
- **Time Complexity**: O(2^n) where n is the number of coins, due to the recursive calls.
- **Space Complexity**: O(n) for the recursion stack depth.

---

### Dynamic Programming - 2D Array

#### Intuition
The recursive solution can be optimized using dynamic programming. We use a 2D array where `dp[i][j]` represents the number of ways to get the amount `j` using the first `i` types of coins. The idea is to build up the solution from smaller amounts using previously computed results.

#### Code

```go
func change(amount int, coins []int) int {
    n := len(coins)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, amount+1)
    }
    // Base case: number of ways to make amount 0 is 1 (use no coins)
    for i := 0; i <= n; i++ {
        dp[i][0] = 1
    }

    for i := 1; i <= n; i++ {
        for j := 1; j <= amount; j++ {
            if coins[i-1] <= j {
                dp[i][j] = dp[i-1][j] + dp[i][j-coins[i-1]]
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    return dp[n][amount]
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n * amount), where n is the length of `coins`.
- **Space Complexity**: O(n * amount), due to the 2D DP array.

---

### Dynamic Programming - 1D Array

#### Intuition
We can further optimize the space usage by realizing that we only need the previous results for the same row and the current row computation can be done in a single dimension using only the prior state values.

#### Code

```go
func change(amount int, coins []int) int {
    dp := make([]int, amount+1)
    dp[0] = 1 // Base case: One way to make 0 amount

    for _, coin := range coins {
        for j := coin; j <= amount; j++ {
            dp[j] += dp[j-coin]
        }
    }
    return dp[amount]
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n * amount), where n is the length of `coins`.
- **Space Complexity**: O(amount), as we use a 1D DP array.

---

By following these approaches, you move from a basic recursive approach that's simple but inefficient, to an optimal dynamic programming solution that efficiently computes the result using iterative combined states.

