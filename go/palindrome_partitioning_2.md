# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
package main

func minCut(s string) int {
	n := len(s)
	isPalindrome := make([][]bool, n)
	for i := range isPalindrome {
		isPalindrome[i] = make([]bool, n)
	}

	// Fill the isPalindrome table
	for length := 1; length <= n; length++ {
		for i := 0; i <= n-length; i++ {
			j := i + length - 1
			if s[i] == s[j] {
				isPalindrome[i][j] = length <= 2 || isPalindrome[i+1][j-1]
			}
		}
	}

	cuts := make([]int, n)
	for i := 0; i < n; i++ {
		if isPalindrome[0][i] {
			cuts[i] = 0
		} else {
			cuts[i] = i
			for j := 0; j < i; j++ {
				if isPalindrome[j+1][i] {
					if cuts[i] > cuts[j]+1 {
						cuts[i] = cuts[j] + 1
					}
				}
			}
		}
	}

	return cuts[n-1]
}
```

## Approach 2: Optimized with Less Space for Palindrome Check

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func minCut(s string) int {
	n := len(s)
	cuts := make([]int, n)
	isPalindrome := make([]bool, n)

	for i := 0; i < n; i++ {
		minCuts := i
		for j := 0; j <= i; j++ {
			if s[j] == s[i] && (i-j <= 1 || isPalindrome[j+1]) {
				isPalindrome[j] = true
				if j == 0 {
					minCuts = 0
				} else {
					if minCuts > cuts[j-1]+1 {
						minCuts = cuts[j-1] + 1
					}
				}
			} else {
				isPalindrome[j] = false
			}
		}
		cuts[i] = minCuts
	}

	return cuts[n-1]
}
```

