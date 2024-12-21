## [Leetcode 322: Coin Change](https://leetcode.com/problems/coin-change/)

### Approaches:
- [Approach 1: Recursive Brute Force](#approach-1-recursive-brute-force)
- [Approach 2: Top-Down Dynamic Programming with Memoization](#approach-2-top-down-dynamic-programming-with-memoization)
- [Approach 3: Bottom-Up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)

---

### Approach 1: Recursive Brute Force

#### Intuition:
The brute force approach involves trying every possible combination of coins to see if it meets the required amount. We recursively try to find the minimum coins needed by reducing the amount by the value of each coin again and again.

#### Code:
```go
func coinChange(coins []int, amount int) int {
	// Recursive helper function to find the minimum number of coins
	var coinChangeRecursive func(amount int) int
	coinChangeRecursive = func(amount int) int {
		// Base cases
		if amount == 0 {
			return 0 // No coins needed for zero amount
		}
		if amount < 0 {
			return -1 // Not possible to get a negative amount
		}

		minCoins := int(^uint(0) >> 1) // Start with a large value for min coins
		for _, coin := range coins {
			res := coinChangeRecursive(amount - coin) // Recur for reduced amount
			if res >= 0 && res < minCoins {
				minCoins = 1 + res // Update the minimum if possible
			}
		}

		// Return the minimum coins found, or -1 if not possible
		if minCoins == int(^uint(0)>>1) {
			return -1
		}
		return minCoins
	}

	return coinChangeRecursive(amount)
}
```

#### Time Complexity:
- Exponential, O(S^n) where S is the amount and n is the number of coins. In the worst case, every recursive call is made with every coin value.

#### Space Complexity:
- O(S) due to the depth of the recursion stack.

---

### Approach 2: Top-Down Dynamic Programming with Memoization

#### Intuition:
We improve the recursive solution by using memoization to store the results of subproblems, avoiding redundant calculations and reducing the number of recursive calls.

#### Code:
```go
func coinChange(coins []int, amount int) int {
	// Memoization table to store the results of subproblems
	memo := make(map[int]int)

	var coinChangeMemo func(amount int) int
	coinChangeMemo = func(amount int) int {
		// Base cases
		if amount == 0 {
			return 0
		}
		if amount < 0 {
			return -1
		}
		
		// Check if the result is already calculated
		if val, found := memo[amount]; found {
			return val
		}
		
		minCoins := int(^uint(0) >> 1)
		for _, coin := range coins {
			res := coinChangeMemo(amount - coin)
			if res >= 0 && res < minCoins {
				minCoins = 1 + res
			}
		}

		// Memoize the calculated result
		if minCoins == int(^uint(0)>>1) {
			memo[amount] = -1
		} else {
			memo[amount] = minCoins
		}

		return memo[amount]
	}

	return coinChangeMemo(amount)
}
```

#### Time Complexity:
- O(S * n), S is the amount and n is the number of coins. Each subproblem is solved only once.

#### Space Complexity:
- O(S), for the memoization table and recursion stack.

---

### Approach 3: Bottom-Up Dynamic Programming

#### Intuition:
The optimal approach uses a bottom-up dynamic programming table where we systematically build up the minimum coins needed for all amounts up to the target amount.

#### Code:
```go
func coinChange(coins []int, amount int) int {
	// DP table initialized with a large number for infinity
	dp := make([]int, amount+1)
	for i := range dp {
		dp[i] = amount + 1
	}
	dp[0] = 0 // No coins needed to make up amount 0

	for i := 1; i <= amount; i++ {
		for _, coin := range coins {
			if i - coin >= 0 {
				dp[i] = min(dp[i], 1 + dp[i - coin])
			}
		}
	}

	// If dp[amount] is still the initialized value, it means it's not possible
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

#### Time Complexity:
- O(S * n), as every sub-amount from 1 to S is computed once using all coins.

#### Space Complexity:
- O(S), as the dp array stores results for each amount up to S.

---

