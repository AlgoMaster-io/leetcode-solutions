# 673. [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)

## Approach 1: Dynamic Programming with Two Arrays

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
func findNumberOfLIS(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    n := len(nums)
    lengths := make([]int, n)
    counts := make([]int, n)
    for i := range lengths {
        lengths[i] = 1
        counts[i] = 1
    }

    for i := 0; i < n; i++ {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                if lengths[j]+1 > lengths[i] {
                    lengths[i] = lengths[j] + 1
                    counts[i] = counts[j]
                } else if lengths[j]+1 == lengths[i] {
                    counts[i] += counts[j]
                }
            }
        }
    }

    maxLength, numberOfLIS := 0, 0
    for _, length := range lengths {
        if length > maxLength {
            maxLength = length
        }
    }
    for i := 0; i < n; i++ {
        if lengths[i] == maxLength {
            numberOfLIS += counts[i]
        }
    }

    return numberOfLIS
}
```

## Approach 2: Optimized Dynamic Programming with Fenwick Tree

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import (
    "sort"
)

type FenwickTree struct {
    lengths []int
    counts  []int
}

func NewFenwickTree(size int) *FenwickTree {
    return &FenwickTree{
        lengths: make([]int, size),
        counts:  make([]int, size),
    }
}

func (ft *FenwickTree) query(index int) (int, int) {
    maxLength, totalCounts := 0, 0
    for ; index > 0; index -= index & -index {
        if ft.lengths[index] > maxLength {
            maxLength = ft.lengths[index]
            totalCounts = ft.counts[index]
        } else if ft.lengths[index] == maxLength {
            totalCounts += ft.counts[index]
        }
    }
    return maxLength, totalCounts
}

func (ft *FenwickTree) update(index, length, count int) {
    for ; index < len(ft.lengths); index += index & -index {
        if ft.lengths[index] < length {
            ft.lengths[index] = length
            ft.counts[index] = count
        } else if ft.lengths[index] == length {
            ft.counts[index] += count
        }
    }
}

func findNumberOfLIS(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    
    offset := 1
    set := make(map[int]struct{})
    for _, num := range nums {
        set[num] = struct{}{}
    }
    
    uniqueNums := make([]int, 0, len(set))
    for num := range set {
        uniqueNums = append(uniqueNums, num)
    }
    sort.Ints(uniqueNums)

    numToIndex := make(map[int]int)
    for i, num := range uniqueNums {
        numToIndex[num] = i + 1
    }

    fenwickTree := NewFenwickTree(len(uniqueNums) + offset)
    maxLength, result := 0, 0

    for _, num := range nums {
        currentIndex := numToIndex[num]
        previousLength, previousCount := fenwickTree.query(currentIndex - 1)
        currentLength := previousLength + 1
        currentCount := max(previousCount, 1)

        fenwickTree.update(currentIndex, currentLength, currentCount)

        if currentLength > maxLength {
            maxLength = currentLength
            result = currentCount
        } else if currentLength == maxLength {
            result += currentCount
        }
    }

    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

