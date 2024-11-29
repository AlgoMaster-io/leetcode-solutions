# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n * sqrt(n))
// Space Complexity: O(n)
package main

import (
	"math"
)

func numSquares(n int) int {
	dp := make([]int, n+1)
	dp[0] = 0 // Zero can be represented by zero squares

	for i := 1; i <= n; i++ {
		min := math.MaxInt32
		for j := 1; j*j <= i; j++ {
			min = minInt(min, dp[i-j*j]+1)
		}
		dp[i] = min
	}

	return dp[n] // Result is stored in dp[n]
}

func minInt(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func numSquares(n int) int {
	queue := []int{0}
	visited := make([]bool, n+1)
	visited[0] = true

	level := 0
	for len(queue) > 0 {
		size := len(queue)
		level++
		for i := 0; i < size; i++ {
			sum := queue[0]
			queue = queue[1:]
			for j := 1; sum+j*j <= n; j++ {
				next := sum + j*j
				if next == n {
					return level
				}
				if !visited[next] {
					queue = append(queue, next)
					visited[next] = true
				}
			}
		}
	}

	return level
}
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
go
```go
// Time Complexity: O(sqrt(n))
// Space Complexity: O(1)
package main

import (
	"math"
)

func numSquares(n int) int {
	// Check if n is a perfect square
	if isPerfectSquare(n) {
		return 1
	}

	// Check the result is 4 using Legendre's Theorem
	for n%4 == 0 {
		n /= 4
	}
	if n%8 == 7 {
		return 4
	}

	// Check if it can be decomposed into sum of two squares
	for i := 1; i*i <= n; i++ {
		if isPerfectSquare(n - i*i) {
			return 2
		}
	}

	return 3
}

func isPerfectSquare(n int) bool {
	s := int(math.Sqrt(float64(n)))
	return s*s == n
}
```

