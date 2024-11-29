# 424. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func characterReplacement(s string, k int) int {
	maxLength := 0

	// Iterate through every possible substring
	for i := 0; i < len(s); i++ {
		freq := make([]int, 26)
		maxFreq := 0

		for j := i; j < len(s); j++ {
			freq[s[j]-'A']++
			if freq[s[j]-'A'] > maxFreq {
				maxFreq = freq[s[j]-'A']
			}

			// Check if the current substring can be made uniform
			if (j-i+1)-maxFreq <= k {
				if (j - i + 1) > maxLength {
					maxLength = j - i + 1
				}
			}
		}
	}

	return maxLength
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func characterReplacement(s string, k int) int {
	freq := make([]int, 26)
	maxFreq := 0
	maxLength := 0
	left := 0

	// Sliding window
	for right := 0; right < len(s); right++ {
		freq[s[right]-'A']++
		if freq[s[right]-'A'] > maxFreq {
			maxFreq = freq[s[right]-'A']
		}

		// If the remaining characters exceed k, shrink the window
		if (right-left+1)-maxFreq > k {
			freq[s[left]-'A']--
			left++
		}

		// Update maxLength
		if (right - left + 1) > maxLength {
			maxLength = right - left + 1
		}
	}

	return maxLength
}
```

