# 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "math"

func minCostClimbingStairs(cost []int) int {
	memo := make([]int, len(cost))
	return int(math.Min(float64(minCost(len(cost)-1, cost, memo)), float64(minCost(len(cost)-2, cost, memo))))
}

func minCost(i int, cost, memo []int) int {
	if i < 0 {
		return 0 // Base case: No cost for negative steps
	}
	if memo[i] != 0 {
		return memo[i] // Return precomputed cost if available
	}

	// Minimum cost to reach this step
	costToReach := cost[i] + int(math.Min(float64(minCost(i-1, cost, memo)), float64(minCost(i-2, cost, memo))))

	memo[i] = costToReach // Store computed result in memo array
	return costToReach
}
```

## Approach 2: Dynamic Programming

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "math"

func minCostClimbingStairs(cost []int) int {
	n := len(cost)
	dp := make([]int, n+1)

	// Base cases: No cost to start at either step 0 or step 1
	dp[0] = 0
	dp[1] = 0

	// Fill the dp array with minimum cost to reach each step
	for i := 2; i <= n; i++ {
		dp[i] = int(math.Min(float64(dp[i-1]+cost[i-1]), float64(dp[i-2]+cost[i-2])))
	}

	return dp[n] // Minimum cost to reach the top
}
```

## Approach 3: Dynamic Programming with Constant Space

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "math"

func minCostClimbingStairs(cost []int) int {
	n := len(cost)
	prev1 := 0
	prev2 := 0

	// Iterate through cost array and calculate minimum cost dynamically
	for i := 2; i <= n; i++ {
		current := int(math.Min(float64(prev1+cost[i-1]), float64(prev2+cost[i-2])))
		prev2 = prev1
		prev1 = current
	}

	return prev1 // Minimum cost to reach the top
}
```


