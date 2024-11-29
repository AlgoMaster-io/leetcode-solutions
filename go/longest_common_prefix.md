# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
go
```go
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
package main

import (
	"strings"
)

func longestCommonPrefix(strs []string) string {
	if strs == nil || len(strs) == 0 {
		return ""
	}

	// Start with the first string as the prefix
	prefix := strs[0]

	// Compare the prefix with each string in the array
	for i := 1; i < len(strs); i++ {
		for strings.Index(strs[i], prefix) != 0 {
			// Shorten the prefix until it matches the start of the current string
			prefix = prefix[:len(prefix)-1]
			if prefix == "" {
				return "" // No common prefix
			}
		}
	}

	return prefix
}
```

## Approach 2: Vertical Scanning

### Solution
go
```go
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(1)
package main

func longestCommonPrefix(strs []string) string {
	if strs == nil || len(strs) == 0 {
		return ""
	}

	// Iterate over each character of the first string
	for i := 0; i < len(strs[0]); i++ {
		c := strs[0][i]

		// Compare this character with the same position in all strings
		for j := 1; j < len(strs); j++ {
			// If characters don't match or exceed the string's length
			if i >= len(strs[j]) || strs[j][i] != c {
				return strs[0][:i] // Return the common prefix
			}
		}
	}

	return strs[0] // The entire first string is the common prefix
}
```

## Approach 3: Divide and Conquer

### Solution
go
```go
// Time Complexity: O(S) where S is the sum of all characters in the strings
// Space Complexity: O(m * log n) where m is the average string length
package main

func longestCommonPrefix(strs []string) string {
	if strs == nil || len(strs) == 0 {
		return ""
	}

	return lcpUtil(strs, 0, len(strs)-1)
}

func lcpUtil(strs []string, left, right int) string {
	if left == right {
		return strs[left] // Base case: single string
	}

	// Divide the array into two halves
	mid := (left + right) / 2
	lcpLeft := lcpUtil(strs, left, mid)
	lcpRight := lcpUtil(strs, mid+1, right)

	// Merge results
	return commonPrefix(lcpLeft, lcpRight)
}

func commonPrefix(left, right string) string {
	minLength := min(len(left), len(right))
	for i := 0; i < minLength; i++ {
		if left[i] != right[i] {
			return left[:i]
		}
	}
	return left[:minLength]
}

func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```

## Approach 4: Binary Search

### Solution
go
```go
// Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
// Space Complexity: O(1)
package main

func longestCommonPrefix(strs []string) string {
	if strs == nil || len(strs) == 0 {
		return ""
	}

	minLen := findMinLength(strs)
	low, high := 1, minLen
	for low <= high {
		mid := (low + high) / 2
		if isCommonPrefix(strs, mid) {
			low = mid + 1 // Try for a longer prefix
		} else {
			high = mid - 1 // Try for a shorter prefix
		}
	}

	return strs[0][:((low + high) / 2)]
}

func findMinLength(strs []string) int {
	minLen := len(strs[0])
	for _, str := range strs {
		if len(str) < minLen {
			minLen = len(str)
		}
	}
	return minLen
}

func isCommonPrefix(strs []string, length int) bool {
	prefix := strs[0][:length]
	for i := 1; i < len(strs); i++ {
		if !strings.HasPrefix(strs[i], prefix) {
			return false
		}
	}
	return true
}
```

