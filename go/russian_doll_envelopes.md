# 354. [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Approach 1: Dynamic Programming

### Solution
Go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import "sort"

func maxEnvelopes(envelopes [][]int) int {
    n := len(envelopes)
    if n == 0 {
        return 0
    }

    // Sort envelopes by width; if width is the same, sort by height descending
    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })

    dp := make([]int, n)
    for i := range dp {
        dp[i] = 1
    }

    maxEnvelopes := 0

    // Dynamic programming to find the maximum number of envelopes
    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            // Check if j can fit into i
            if envelopes[j][1] < envelopes[i][1] && envelopes[j][0] < envelopes[i][0] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxEnvelopes = max(maxEnvelopes, dp[i])
    }

    return maxEnvelopes
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```

## Approach 2: Patience Sorting and Longest Increasing Subsequence (LIS)

### Solution
Go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import (
    "sort"
)

func maxEnvelopes(envelopes [][]int) int {
    n := len(envelopes)
    if n == 0 {
        return 0
    }

    // Sort envelopes by width; if width is the same, sort by height descending
    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })

    // Array to hold our increasing sequence of heights
    lis := make([]int, 0)

    for _, envelope := range envelopes {
        height := envelope[1]

        // Perform binary search to find the correct position to replace or add
        pos := binarySearch(lis, height)

        // Add or replace the value at the identified position
        if pos < len(lis) {
            lis[pos] = height
        } else {
            lis = append(lis, height)
        }
    }

    return len(lis)
}

func binarySearch(lis []int, key int) int {
    low, high := 0, len(lis)-1
    for low <= high {
        mid := low + (high-low)/2
        if lis[mid] < key {
            low = mid + 1
        } else {
            high = mid - 1
        }
    }
    return low
}
```

