# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "fmt"

func main() {
	fmt.Println(climbStairs(5)) // Example usage
}

var memo = make(map[int]int)

func climbStairs(n int) int {
	if n <= 2 {
		return n
	}
	if val, found := memo[n]; found {
		return val
	}
	// Recurrence relation: f(n) = f(n-1) + f(n-2)
	result := climbStairs(n-1) + climbStairs(n-2)
	memo[n] = result // Store the result in a map to avoid recalculating
	return result
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "fmt"

func main() {
	fmt.Println(climbStairs(5)) // Example usage
}

func climbStairs(n int) int {
	if n <= 2 {
		return n
	}

	dp := make([]int, n+1)
	dp[1] = 1
	dp[2] = 2

	// Build the dp array with previous solutions
	for i := 3; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}

	return dp[n] // The answer is stored in the last element
}
```

## Approach 3: Optimized Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "fmt"

func main() {
	fmt.Println(climbStairs(5)) // Example usage
}

func climbStairs(n int) int {
	if n <= 2 {
		return n
	}

	first, second := 1, 2 // Represents dp[i-2], dp[i-1]
	var current int

	// Use two variables to avoid full dp array and update them iteratively
	for i := 3; i <= n; i++ {
		current = first + second // Current step number is sum of previous two steps
		first = second           // Move second to first
		second = current         // Update second to the current
	}

	return current // Result is stored in current
}
```

