# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n * m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
package main

import "strings"

func repeatedStringMatch(A string, B string) int {
    repeatedA := A
    repeatCount := 1

    // Repeat A until its length is at least the length of B
    for len(repeatedA) < len(B) {
        repeatedA += A
        repeatCount++
    }

    // Check if B is a substring of the repeated A
    if strings.Contains(repeatedA, B) {
        return repeatCount
    }

    // Add one more repetition just in case B starts towards the end
    repeatedA += A
    if strings.Contains(repeatedA, B) {
        return repeatCount + 1
    }

    return -1 // B is not a substring of repeated A
}
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
go
```go
// Time Complexity: O(n + m), where n is the length of A and m is the length of B
// Space Complexity: O(n + m)
package main

import "strings"

func repeatedStringMatch(A string, B string) int {
    m := len(A)
    n := len(B)
    maxRepeat := (n / m) + 2 // Only need to repeat at most this many times

    repeatedA := ""
    for i := 0; i < maxRepeat; i++ {
        repeatedA += A
        // Check if B is a substring of the current repeated A
        if strings.Contains(repeatedA, B) {
            return i + 1
        }
    }

    return -1 // B is not a substring of repeated A
}
```

