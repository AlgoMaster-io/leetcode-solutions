# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(1) (ignoring the space required for sorting)
import "sort"

func maximumGap(nums []int) int {
    if nums == nil || len(nums) < 2 {
        return 0
    }

    // Sort the array
    sort.Ints(nums)

    maxGap := 0

    // Calculate the maximum gap between consecutive elements
    for i := 1; i < len(nums); i++ {
        maxGap = max(maxGap, nums[i] - nums[i-1])
    }

    return maxGap
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
import "math"

func maximumGap(nums []int) int {
    if nums == nil || len(nums) < 2 {
        return 0
    }

    min := math.MaxInt64
    max := math.MinInt64
    for _, num := range nums {
        if num < min {
            min = num
        }
        if num > max {
            max = num
        }
    }

    // Calculate the gap size and number of buckets
    n := len(nums)
    gap := int(math.Ceil(float64(max-min) / float64(n-1)))
    bucketMin := make([]int, n-1)
    bucketMax := make([]int, n-1)
    for i := range bucketMin {
        bucketMin[i] = math.MaxInt64
        bucketMax[i] = math.MinInt64
    }

    // Place each number into a bucket
    for _, num := range nums {
        if num == min || num == max {
            continue
        }
        index := (num - min) / gap
        if num < bucketMin[index] {
            bucketMin[index] = num
        }
        if num > bucketMax[index] {
            bucketMax[index] = num
        }
    }

    // Calculate the maximum gap
    maxGap, previous := 0, min
    for i := 0; i < n-1; i++ {
        if bucketMin[i] == math.MaxInt64 {
            continue // Skip empty buckets
        }
        maxGap = max(maxGap, bucketMin[i]-previous)
        previous = bucketMax[i]
    }
    maxGap = max(maxGap, max-previous) // Final gap with the max element

    return maxGap
}
```

In the above Go solutions, sorting and calculating the maximum gap follows the same logic as the Java approaches, making the implementations straightforward to translate.

