# 518. [Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approach 1: Recursive Approach

### Solution
```go
// Time Complexity: O(2^n), where n is the number of coins
// Space Complexity: O(n)
package main

func change(amount int, coins []int) int {
    return countWays(coins, len(coins), amount)
}

// Recursive function to count ways to make change
func countWays(coins []int, numOfCoins, amount int) int {
    // Base case: If amount is 0, there is 1 solution (to use no coins)
    if amount == 0 {
        return 1
    }
    
    // If amount is less than 0, there's no solution
    if amount < 0 {
        return 0
    }
    
    // If there are no coins and amount is more than 0, no solution
    if numOfCoins <= 0 && amount > 0 {
        return 0
    }

    // Count is the sum of solutions including coins[numOfCoins-1]
    // and excluding coins[numOfCoins-1]
    return countWays(coins, numOfCoins-1, amount) + countWays(coins, numOfCoins, amount-coins[numOfCoins-1])
}
```

## Approach 2: Dynamic Programming (2D Array)

### Solution
```go
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(n * m)
package main

func change(amount int, coins []int) int {
    // dp[i][j] means the number of ways to make change for amount j using first i types of coins
    dp := make([][]int, len(coins)+1)
    for i := 0; i <= len(coins); i++ {
        dp[i] = make([]int, amount+1)
        dp[i][0] = 1 // Base case: If amount is 0, then there's exactly one way to make change: select no coin
    }

    // Fill the dp array
    for i := 1; i <= len(coins); i++ {
        for j := 1; j <= amount; j++ {
            // If we don't pick the coin, we have dp[i-1][j] possible ways
            dp[i][j] = dp[i-1][j]

            // If we pick the coin, we add the ways we can make up the amount with this coin
            if j >= coins[i-1] {
                dp[i][j] += dp[i][j-coins[i-1]]
            }
        }
    }

    return dp[len(coins)][amount]
}
```

## Approach 3: Dynamic Programming (1D Array)

### Solution
```go
// Time Complexity: O(n * m), where n is the number of coins and m is the amount
// Space Complexity: O(m)
package main

func change(amount int, coins []int) int {
    // dp[j] means the number of ways to make change for amount j
    dp := make([]int, amount+1)
    dp[0] = 1 // Base case: one way to make zero amount

    // Traverse each coin
    for _, coin := range coins {
        // Update dp array by considering the current coin
        for j := coin; j <= amount; j++ {
            dp[j] += dp[j-coin]
        }
    }

    return dp[amount]
}
```

