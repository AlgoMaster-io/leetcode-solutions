# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
go
```go
// Time Complexity: O(k^n)
// Space Complexity: O(k)
package main

import "math"

func distributeCookies(cookies []int, k int) int {
	return helper(cookies, make([]int, k), 0)
}

func helper(cookies []int, distribution []int, index int) int {
	if index == len(cookies) {
		max := math.MinInt32
		for _, d := range distribution {
			if d > max {
				max = d
			}
		}
		return max
	}

	minUnfairness := math.MaxInt32
	for i := range distribution {
		distribution[i] += cookies[index]
		unfairness := helper(cookies, distribution, index+1)
		if unfairness < minUnfairness {
			minUnfairness = unfairness
		}
		distribution[i] -= cookies[index]
	}
	return minUnfairness
}
```

## Approach 2: Backtracking with Pruning

### Solution
go
```go
// Time Complexity: O(k * 2^n)
// Space Complexity: O(k)
package main

import "math"

func distributeCookies(cookies []int, k int) int {
	distribution := make([]int, k)
	return helper(cookies, distribution, 0, 0, math.MaxInt32)
}

func helper(cookies []int, distribution []int, index, currentMax, minUnfairness int) int {
	if index == len(cookies) {
		return int(math.Min(float64(minUnfairness), float64(currentMax)))
	}

	for i := range distribution {
		distribution[i] += cookies[index]
		unfairness := helper(cookies, distribution, index+1, int(math.Max(float64(currentMax), float64(distribution[i]))), minUnfairness)
		distribution[i] -= cookies[index]

		if distribution[i] == 0 { // Optimizing to prevent unnecessary states
			break
		}
	}
	return minUnfairness
}
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
go
```go
// Time Complexity: O(n*2^n)
// Space Complexity: O(2^n)
package main

import "math"

func distributeCookies(cookies []int, k int) int {
	n := len(cookies)
	sum := make([]int, 1<<n)

	// Precompute the sum of each subset
	for i := 0; i < (1 << n); i++ {
		for j := 0; j < n; j++ {
			if i&(1<<j) != 0 {
				sum[i] += cookies[j]
			}
		}
	}

	// dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
	dp := make([][]int, k+1)
	for i := range dp {
		dp[i] = make([]int, 1<<n)
		for j := range dp[i] {
			dp[i][j] = math.MaxInt32
		}
	}

	dp[0][0] = 0

	for i := 1; i <= k; i++ {
		for mask := 0; mask < (1 << n); mask++ {
			for submask := mask; submask > 0; submask = (submask - 1) & mask {
				dp[i][mask] = int(math.Min(float64(dp[i][mask]), float64(math.Max(float64(dp[i-1][mask^submask]), float64(sum[submask])))))
			}
		}
	}

	return dp[k][(1<<n)-1]
}
```

