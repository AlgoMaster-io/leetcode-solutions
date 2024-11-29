# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array
import (
	"reflect"
)

func findAnagrams(s string, p string) []int {
	result := []int{}
	pFreq := make([]int, 26)

	// Frequency count for string p
	for _, c := range p {
		pFreq[c-'a']++
	}

	// Check every substring of length p in s
	for i := 0; i <= len(s)-len(p); i++ {
		sFreq := make([]int, 26)

		// Count frequency of the current substring
		for j := i; j < i+len(p); j++ {
			sFreq[s[j]-'a']++
		}

		// Compare frequencies
		if reflect.DeepEqual(sFreq, pFreq) {
			result = append(result, i)
		}
	}

	return result
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
go
```go
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)
import (
	"reflect"
)

func findAnagrams(s string, p string) []int {
	result := []int{}
	pFreq := make([]int, 26)
	sFreq := make([]int, 26)

	// Frequency count for string p
	for _, c := range p {
		pFreq[c-'a']++
	}

	left, right := 0, 0

	// Sliding window over s
	for right < len(s) {
		// Add the current character to the window
		sFreq[s[right]-'a']++

		// Check if the window size matches p
		if right-left+1 == len(p) {
			// Compare frequencies
			if reflect.DeepEqual(sFreq, pFreq) {
				result = append(result, left)
			}

			// Remove the leftmost character from the window
			sFreq[s[left]-'a']--
			left++
		}

		right++
	}

	return result
}
```

