# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

import (
	"fmt"
	"math"
)

func minWindow(s string, t string) string {
	if len(t) > len(s) {
		return ""
	}

	result := ""
	minLength := math.MaxInt32

	for i := 0; i < len(s); i++ {
		for j := i; j < len(s); j++ {
			sub := s[i : j+1]

			if containsAll(sub, t) {
				if len(sub) < minLength {
					minLength = len(sub)
					result = sub
				}
			}
		}
	}

	return result
}

func containsAll(sub string, t string) bool {
	charCount := make(map[rune]int)

	for _, c := range t {
		charCount[c]++
	}

	for _, c := range sub {
		if _, exists := charCount[c]; exists {
			charCount[c]--
			if charCount[c] == 0 {
				delete(charCount, c)
			}
		}
	}

	return len(charCount) == 0
}

func main() {
	s := "ADOBECODEBANC"
	t := "ABC"
	fmt.Println(minWindow(s, t)) // Output: "BANC"
}
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import (
	"fmt"
	"math"
)

func minWindow(s string, t string) string {
	if len(t) > len(s) {
		return ""
	}

	tFreq := make(map[rune]int)
	for _, c := range t {
		tFreq[c]++
	}

	windowFreq := make(map[rune]int)
	left, right, matched := 0, 0, 0
	minLength := math.MaxInt32
	start := 0

	for right < len(s) {
		c := rune(s[right])
		windowFreq[c]++

		if tFreq[c] > 0 && windowFreq[c] == tFreq[c] {
			matched++
		}

		for matched == len(tFreq) {
			if right-left+1 < minLength {
				minLength = right - left + 1
				start = left
			}

			leftChar := rune(s[left])
			if tFreq[leftChar] > 0 && windowFreq[leftChar] == tFreq[leftChar] {
				matched--
			}
			windowFreq[leftChar]--
			if windowFreq[leftChar] == 0 {
				delete(windowFreq, leftChar)
			}
			left++
		}

		right++
	}

	if minLength == math.MaxInt32 {
		return ""
	}
	return s[start : start+minLength]
}

func main() {
	s := "ADOBECODEBANC"
	t := "ABC"
	fmt.Println(minWindow(s, t)) // Output: "BANC"
}
```

