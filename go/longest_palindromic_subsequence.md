# 516. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approach 1: Recursive Solution with Memoization

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
package main

func longestPalindromeSubseq(s string) int {
	n := len(s)
	memo := make([][]int, n)
	for i := range memo {
		memo[i] = make([]int, n)
	}
	return longestPalindromeSubseqHelper(s, 0, n-1, memo)
}

func longestPalindromeSubseqHelper(s string, start, end int, memo [][]int) int {
	if start == end {
		return 1
	}
	if start > end {
		return 0
	}
	if memo[start][end] != 0 {
		return memo[start][end]
	}
	if s[start] == s[end] {
		memo[start][end] = 2 + longestPalindromeSubseqHelper(s, start+1, end-1, memo)
	} else {
		memo[start][end] = max(longestPalindromeSubseqHelper(s, start+1, end, memo),
			longestPalindromeSubseqHelper(s, start, end-1, memo))
	}
	return memo[start][end]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
package main

func longestPalindromeSubseq(s string) int {
	n := len(s)
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, n)
	}

	for i := 0; i < n; i++ {
		dp[i][i] = 1
	}

	for length := 2; length <= n; length++ {
		for start := 0; start <= n-length; start++ {
			end := start + length - 1
			if s[start] == s[end] {
				dp[start][end] = dp[start+1][end-1] + 2
			} else {
				dp[start][end] = max(dp[start+1][end], dp[start][end-1])
			}
		}
	}

	return dp[0][n-1]
}
```

## Approach 3: Optimized Dynamic Programming with Reduced Space

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func longestPalindromeSubseq(s string) int {
	n := len(s)
	dp := make([]int, n)
	prevDp := make([]int, n)

	for i := 0; i < n; i++ {
		dp[i] = 1
	}

	for length := 2; length <= n; length++ {
		for start := 0; start <= n-length; start++ {
			end := start + length - 1
			temp := dp[start]

			if s[start] == s[end] {
				dp[start] = prevDp[start+1] + 2
			} else {
				dp[start] = max(dp[start+1], prevDp[start])
			}

			prevDp[start] = temp
		}
	}

	return dp[0]
}
```


