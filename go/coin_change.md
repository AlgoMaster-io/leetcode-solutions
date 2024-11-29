# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
go
```go
// Time Complexity: O(S^n) where S is the amount and n is the number of coins
// Space Complexity: O(S) for the recursion stack
package main

import (
	"math"
)

func coinChange(coins []int, amount int) int {
	if amount == 0 {
		return 0
	}
	result := helper(coins, amount)
	if result == math.MaxInt32 {
		return -1
	}
	return result
}

func helper(coins []int, remainder int) int {
	if remainder < 0 {
		return math.MaxInt32
	}
	if remainder == 0 {
		return 0
	}

	minCoins := math.MaxInt32
	for _, coin := range coins {
		res := helper(coins, remainder-coin)
		// Avoid integer overflow here and take minimum
		if res != math.MaxInt32 {
			minCoins = min(minCoins, res+1)
		}
	}
	return minCoins
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
go
```go
// Time Complexity: O(S * n)
// Space Complexity: O(S)
package main

import (
	"math"
)

func coinChange(coins []int, amount int) int {
	dp := make([]int, amount+1)

	// Initialize the dp array with a max value
	for i := 1; i <= amount; i++ {
		dp[i] = amount + 1
	}
	dp[0] = 0

	for i := 1; i <= amount; i++ {
		for _, coin := range coins {
			if i-coin >= 0 {
				dp[i] = min(dp[i], dp[i-coin]+1)
			}
		}
	}

	// If dp[amount] is still amount + 1, no solution found
	if dp[amount] > amount {
		return -1
	}
	return dp[amount]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## Approach 3: Dynamic Programming - Top Down with Memoization

### Solution
go
```go
// Time Complexity: O(S * n)
// Space Complexity: O(S)
package main

import (
	"math"
)

var memo []int

func coinChange(coins []int, amount int) int {
	memo = make([]int, amount+1)
	for i := range memo {
		memo[i] = -1
	}
	memo[0] = 0
	return coinChangeRecursive(coins, amount)
}

func coinChangeRecursive(coins []int, remaining int) int {
	if remaining < 0 {
		return -1
	}
	if memo[remaining] != -1 {
		return memo[remaining]
	}

	minCoins := math.MaxInt32
	for _, coin := range coins {
		res := coinChangeRecursive(coins, remaining-coin)

		if res >= 0 {
			minCoins = min(minCoins, res+1)
		}
	}

	if minCoins == math.MaxInt32 {
		memo[remaining] = -1
	} else {
		memo[remaining] = minCoins
	}
	return memo[remaining]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

