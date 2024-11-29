# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func isSubsequence(s string, t string) bool {
    sIndex := 0 // Pointer for string `s`
    tIndex := 0 // Pointer for string `t`

    // Traverse both strings
    for sIndex < len(s) && tIndex < len(t) {
        if s[sIndex] == t[tIndex] {
            sIndex++ // Move pointer in `s` if characters match
        }
        tIndex++ // Always move pointer in `t`
    }

    // If sIndex has reached the end of `s`, it's a subsequence
    return sIndex == len(s)
}
```

## Approach 2: Iterative with Character Search

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main
import "strings"

func isSubsequence(s string, t string) bool {
    index := -1 // Stores the last found position of characters

    // Iterate through each character in `s`
    for _, c := range s {
        // Find the next occurrence of `c` in `t` after the previous index
        index = strings.Index(t[index+1:], string(c))
        if index == -1 {
            // If character not found, `s` is not a subsequence of `t`
            return false
        }
        index += index + 1 // Update index to point to the correct position in t
    }

    // All characters of `s` found in `t` in order
    return true
}
```

## Approach 3: Binary Search (For Multiple Queries)

Use this approach when checking multiple subsequences against the same string t.

### Solution
```go
// Time Complexity: O(n + m log k) for preprocessing + query
// Space Complexity: O(n)
package main

import (
    "sort"
)

func isSubsequence(s string, t string) bool {
    // Preprocess `t`: Create a map of character positions
    charPositions := make(map[rune][]int)
    for i, ch := range t {
        charPositions[ch] = append(charPositions[ch], i)
    }

    lastPosition := -1 // Tracks the position of the last matched character

    // Check each character in `s`
    for _, ch := range s {
        positions, found := charPositions[ch]
        if !found {
            return false // If `ch` not in `t`, return false
        }

        // Perform binary search to find the next valid position
        nextPosition := sort.SearchInts(positions, lastPosition+1)
        if nextPosition == len(positions) {
            return false // No valid position found
        }

        lastPosition = positions[nextPosition] // Update lastPosition
    }

    return true
}
```

