# [Leetcode 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Table of Contents
- [Naive Approach: Brute Force](#naive-approach-brute-force)
- [Optimized Approach: Sliding Window with Frequency Count](#optimized-approach-sliding-window-with-frequency-count)

### Naive Approach: Brute Force

#### Intuition:
The problem can be solved by checking every possible substring of length `p` in `s` and comparing it to the sorted version of `p`. If the sorted substrings match, then they are anagrams.

#### Approach:
1. Generate every substring of `s` that has the same length as `p`.
2. Sort each substring and compare it with the sorted version of `p`.
3. If they match, add the starting index of the substring to the result list.

#### Time Complexity:
- O((n-m+1) * m log m) where `n` is the length of `s` and `m` is the length of `p`.

#### Space Complexity:
- O(m) for storing the sorted version of `p` and each substring of `s`.

```go
package main

import (
	"sort"
	"strings"
)

func findAnagrams(s string, p string) []int {
	var result []int
	pLen := len(p)
	sortedP := sortString(p)

	for i := 0; i <= len(s)-pLen; i++ {
		substring := s[i : i+pLen]
		if sortString(substring) == sortedP {
			result = append(result, i)
		}
	}
	return result
}

func sortString(s string) string {
	sSlice := strings.Split(s, "")
	sort.Strings(sSlice)
	return strings.Join(sSlice, "")
}
```

### Optimized Approach: Sliding Window with Frequency Count

#### Intuition:
Utilize a sliding window of size `p` across `s` and maintain a frequency count of characters. By comparing the frequency counts of the current window and `p`, we can determine whether the current window is an anagram of `p`.

#### Approach:
1. Create frequency count arrays for `p` and the first window of `s` of length `p`.
2. Compare the two frequency arrays; if they match, the window is an anagram.
3. Slide the window one character forward by updating the counts: decrease the count of the outgoing character and increase the count of the incoming character.
4. Repeat the comparison for each window position.

#### Time Complexity:
- O(n) where `n` is the length of `s`.

#### Space Complexity:
- O(1) as the space is independent of input size (fixed array sizes for frequency counts).

```go
package main

func findAnagrams(s string, p string) []int {
	var result []int
	sLen, pLen := len(s), len(p)

	if sLen < pLen {
		return result
	}

	var pCount, sCount [26]int

	// Initialize frequency counts for p and the first window of s
	for i := 0; i < pLen; i++ {
		pCount[p[i]-'a']++
		sCount[s[i]-'a']++
	}

	// Function to check if two frequency arrays are the same
	isAnagram := func(a, b [26]int) bool {
		for i := 0; i < 26; i++ {
			if a[i] != b[i] {
				return false
			}
		}
		return true
	}

	if isAnagram(pCount, sCount) {
		result = append(result, 0)
	}

	// Slide the window over s, updating counts and checking for anagrams
	for i := pLen; i < sLen; i++ {
		sCount[s[i]-'a']++
		sCount[s[i-pLen]-'a']--

		if isAnagram(pCount, sCount) {
			result = append(result, i-pLen+1)
		}
	}

	return result
}
```

This sliding window approach efficiently finds all anagrams with the use of frequency arrays, significantly reducing the computation time compared to the brute force method.

