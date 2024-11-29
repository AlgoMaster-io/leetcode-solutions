# 528. [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

## Approach 1: Linear Search for Picking (Alternative to Binary Search)

### Solution
```go
// Time Complexity: O(n) for initialization, O(n) for each pick
// Space Complexity: O(n)
import (
    "math/rand"
    "time"
)

type Solution struct {
    prefixSums []int
}

func Constructor(w []int) Solution {
    prefixSums := make([]int, len(w))
    prefixSums[0] = w[0]
    for i := 1; i < len(w); i++ {
        prefixSums[i] = prefixSums[i-1] + w[i]
    }
    rand.Seed(time.Now().UnixNano())
    return Solution{prefixSums}
}

func (s *Solution) PickIndex() int {
    target := rand.Intn(s.prefixSums[len(s.prefixSums)-1]) + 1
    for i := 0; i < len(s.prefixSums); i++ {
        if target <= s.prefixSums[i] {
            return i
        }
    }
    return -1 // Should not reach here
}
```

## Approach 2: Prefix Sum Array and Binary Search

### Solution
```go
// Time Complexity: O(n) for initialization, O(log n) for each pick
// Space Complexity: O(n)
import (
    "math/rand"
    "time"
)

type Solution struct {
    prefixSums []int
}

func Constructor(w []int) Solution {
    prefixSums := make([]int, len(w))
    prefixSums[0] = w[0]
    for i := 1; i < len(w); i++ {
        prefixSums[i] = prefixSums[i-1] + w[i]
    }
    rand.Seed(time.Now().UnixNano())
    return Solution{prefixSums}
}

func (s *Solution) PickIndex() int {
    target := rand.Intn(s.prefixSums[len(s.prefixSums)-1]) + 1
    start, end := 0, len(s.prefixSums)-1
    for start < end {
        mid := start + (end-start)/2
        if s.prefixSums[mid] < target {
            start = mid + 1
        } else {
            end = mid
        }
    }
    return start
}
```

