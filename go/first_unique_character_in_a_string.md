# 387. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

## Approach 1: HashMap for Character Frequency

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func firstUniqChar(s string) int {
    countMap := make(map[rune]int)

    // Count the frequency of each character
    for _, c := range s {
        countMap[c]++
    }

    // Find the first character with frequency 1
    for i, c := range s {
        if countMap[c] == 1 {
            return i
        }
    }

    return -1 // No unique character found
}
```

## Approach 2: Array for Character Frequency

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func firstUniqChar(s string) int {
    charCount := make([]int, 26) // Array to store frequency of each character

    // Count the frequency of each character
    for _, c := range s {
        charCount[c-'a']++
    }

    // Find the first character with frequency 1
    for i, c := range s {
        if charCount[c-'a'] == 1 {
            return i
        }
    }

    return -1 // No unique character found
}
```

## Approach 3: Single Pass with Index Array

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "math"

func firstUniqChar(s string) int {
    index := make([]int, 26) // Array to track occurrences and order of characters
    for i := 0; i < 26; i++ {
        index[i] = -1 // Initialize with -1 (not encountered)
    }

    // Traverse string to fill occurrences and order
    for i, c := range s {
        charIndex := c - 'a'
        if index[charIndex] == -1 {
            index[charIndex] = i // First occurrence
        } else {
            index[charIndex] = -2 // Mark as repeated
        }
    }

    // Find the smallest index of a unique character
    result := math.MaxInt32
    for _, idx := range index {
        if idx >= 0 {
            result = min(result, idx)
        }
    }

    if result == math.MaxInt32 {
        return -1
    }
    return result
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

