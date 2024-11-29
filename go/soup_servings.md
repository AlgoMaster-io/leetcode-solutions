# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
package main

import (
	"fmt"
)

type Solution struct {
	memo map[string]float64
}

func (s *Solution) soupServings(N int) float64 {
	if N >= 5000 {
		return 1.0 // Threshold optimization
	}
	s.memo = make(map[string]float64)
	return s.serve(N, N)
}

func (s *Solution) serve(a, b int) float64 {
	if a <= 0 && b <= 0 {
		return 0.5 // Both soups are finished
	}
	if a <= 0 {
		return 1 // Only soup A finishes
	}
	if b <= 0 {
		return 0 // Only soup B finishes
	}

	key := fmt.Sprintf("%d,%d", a, b)
	if val, found := s.memo[key]; found {
		return val
	}

	probability := 0.25 * (
		s.serve(a-100, b) +
			s.serve(a-75, b-25) +
			s.serve(a-50, b-50) +
			s.serve(a-25, b-75),
	)

	s.memo[key] = probability
	return probability
}
```

## Approach 2: Iterative with Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
package main

func soupServings(N int) float64 {
	if N >= 5000 {
		return 1.0 // Threshold optimization
	}
	N = (N + 24) / 25 // Reduce and round up

	dp := make([][]float64, N+1)
	for i := range dp {
		dp[i] = make([]float64, N+1)
	}

	dp[0][0] = 0.5 // Base case: both soups are empty
	for i := 1; i <= N; i++ {
		dp[i][0] = 0 // Soup B empty first
		dp[0][i] = 1 // Soup A empty first
	}

	for i := 1; i <= N; i++ {
		for j := 1; j <= N; j++ {
			dp[i][j] = 0.25 * (
				getProbability(dp, i-4, j) +
					getProbability(dp, i-3, j-1) +
					getProbability(dp, i-2, j-2) +
					getProbability(dp, i-1, j-3),
			)
		}
	}

	return dp[N][N]
}

func getProbability(dp [][]float64, a, b int) float64 {
	if a <= 0 && b <= 0 {
		return 0.5 // Both finish at the same time
	}
	if a <= 0 {
		return 1 // Only soup A finishes
	}
	if b <= 0 {
		return 0 // Only soup B finishes
	}
	return dp[a][b]
}
```

