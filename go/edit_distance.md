# 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approach 1: Recursive Solution

### Solution
```go
// Time Complexity: O(3^(m+n))
// Space Complexity: O(m+n)
package main

func minDistance(word1, word2 string) int {
	// Helper method to compute edit distance recursively
	return computeDistance(word1, word2, len(word1), len(word2))
}

func computeDistance(word1, word2 string, m, n int) int {
	// If first string is empty, return length of second string
	if m == 0 {
		return n
	}

	// If second string is empty, return length of first string
	if n == 0 {
		return m
	}

	// If characters are the same, continue with next characters
	if word1[m-1] == word2[n-1] {
		return computeDistance(word1, word2, m-1, n-1)
	}

	// Calculate minimum operation from insert, remove, or replace
	return 1 + min(
		computeDistance(word1, word2, m, n-1), // Insert
		min(
			computeDistance(word1, word2, m-1, n),    // Remove
			computeDistance(word1, word2, m-1, n-1), // Replace
		),
	)
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## Approach 2: Dynamic Programming (2D Table)

### Solution
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
package main

func minDistanceDP(word1, word2 string) int {
	m, n := len(word1), len(word2)
	
	// Create DP table
	dp := make([][]int, m+1)
	for i := range dp {
		dp[i] = make([]int, n+1)
	}

	// Initialize base cases
	for i := 0; i <= m; i++ {
		dp[i][0] = i
	}
	for j := 0; j <= n; j++ {
		dp[0][j] = j
	}

	// Fill in DP table
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			if word1[i-1] == word2[j-1] {
				dp[i][j] = dp[i-1][j-1] // Characters match, no new operation
			} else {
				dp[i][j] = 1 + min(
					dp[i-1][j], // Remove
					min(
						dp[i][j-1], // Insert
						dp[i-1][j-1], // Replace
					),
				)
			}
		}
	}

	return dp[m][n]
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```go
// Time Complexity: O(m * n)
// Space Complexity: O(n)
package main

func minDistanceOptimized(word1, word2 string) int {
	m, n := len(word1), len(word2)

	// Create two arrays for dynamic programming, only need two rows at a time
	prev := make([]int, n+1)
	curr := make([]int, n+1)

	// Initialize base case for the first row
	for j := 0; j <= n; j++ {
		prev[j] = j
	}

	// Fill in DP table
	for i := 1; i <= m; i++ {
		curr[0] = i // Base case for the first column
		for j := 1; j <= n; j++ {
			if word1[i-1] == word2[j-1] {
				curr[j] = prev[j-1] // Characters match
			} else {
				curr[j] = 1 + min(
					prev[j], // Remove
					min(
						curr[j-1], // Insert
						prev[j-1], // Replace
					),
				)
			}
		}
		prev, curr = curr, prev
	}

	return prev[n]
}
```


