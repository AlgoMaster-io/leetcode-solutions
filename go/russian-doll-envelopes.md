# [Leetcode Problem 354: Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

## Solutions
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Sorting with Binary Search (Optimized)](#approach-3-sorting-with-binary-search-optimized)

### Approach 1: Brute Force

**Intuition**:  
The idea is to try all combinations by placing each envelope inside another. Since this involves checking each envelope against every other envelope, it becomes a complex problem with `O(n!)` possibilities.

**Time Complexity**:  
O(n^2) - Due to the nested iteration over the list of envelopes.

**Space Complexity**:  
O(1) - No additional space is used, as we're modifying in place.

```go
package main

import "fmt"

func maxEnvelopes(envelopes [][]int) int {
    if len(envelopes) == 0 {
        return 0
    }

    maxCount := 1
    // Exhaustive checking of all possible nesting
    for i := 0; i < len(envelopes); i++ {
        count := 1
        current := envelopes[i]
        for j := 0; j < len(envelopes); j++ {
            if i != j && envelopes[j][0] > current[0] && envelopes[j][1] > current[1] {
                count++
                current = envelopes[j]
            }
        }
        if count > maxCount {
            maxCount = count
        }
    }
    return maxCount
}

func main() {
    envelopes := [][]int{{5,4},{6,4},{6,7},{2,3}}
    fmt.Println(maxEnvelopes(envelopes)) // Expected output: 3
}
```

### Approach 2: Dynamic Programming

**Intuition**:  
Here we employ a modified version of the Longest Increasing Subsequence (LIS) algorithm. First, sort the envelopes by width. For envelopes with the same width, sort by height in descending order. This ensures that we cannot nest envelopes of the same width inside each other. Then, find the LIS based on height.

**Time Complexity**:  
O(n^2) - Due to a nested loop for LIS calculation.

**Space Complexity**:  
O(n) - For the `dp` array used to track the longest increasing subsequence by height.

```go
package main

import (
    "fmt"
    "sort"
)

func maxEnvelopes(envelopes [][]int) int {
    if len(envelopes) == 0 {
        return 0
    }

    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            // Descending by height if width is equal
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })

    // Perform LIS based on heights
    n := len(envelopes)
    dp := make([]int, n)
    maxLen := 1
    for i := range dp {
        dp[i] = 1
    }

    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            if envelopes[i][1] > envelopes[j][1] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxLen = max(maxLen, dp[i])
    }

    return maxLen
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}

func main() {
    envelopes := [][]int{{5,4},{6,4},{6,7},{2,3}}
    fmt.Println(maxEnvelopes(envelopes)) // Expected output: 3
}
```

### Approach 3: Sorting with Binary Search (Optimized)

**Intuition**:  
This approach combines sorting with binary search to achieve optimal performance. Envelopes are first sorted by width (and by height in descending order for equal widths) to enable a straightforward calculation of the LIS based on height using a binary search optimized technique.

**Time Complexity**:  
O(n log n) - Sorting takes `O(n log n)` and building the LIS with binary search also gives `O(n log n)`.

**Space Complexity**:  
O(n) - For the `lis` slice that holds the current longest increasing subsequence of heights.

```go
package main

import (
    "fmt"
    "sort"
)

func maxEnvelopes(envelopes [][]int) int {
    if len(envelopes) == 0 {
        return 0
    }

    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })

    lis := []int{}
    for _, env := range envelopes {
        height := env[1]
        idx := sort.Search(len(lis), func(i int) bool { return lis[i] >= height })
        if idx < len(lis) {
            lis[idx] = height
        } else {
            lis = append(lis, height)
        }
    }

    return len(lis)
}

func main() {
    envelopes := [][]int{{5,4},{6,4},{6,7},{2,3}}
    fmt.Println(maxEnvelopes(envelopes)) // Expected output: 3
}
```

In the optimized approach, sorting by descending height for equal width ensures we do not mistakenly consider envelopes with the same width and allows us to apply the LIS technique effectively.

