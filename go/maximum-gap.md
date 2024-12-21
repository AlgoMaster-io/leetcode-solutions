# [Maximum Gap - Leetcode 164](https://leetcode.com/problems/maximum-gap/)

## Approaches

1. [Naive Approach: Sorting](#naive-approach-sorting)
2. [Optimal Approach: Bucket Sort](#optimal-approach-bucket-sort)

### Naive Approach: Sorting

**Intuition**

The simplest way to find the maximum gap is to sort the array and then find the maximum difference between consecutive elements. This approach takes advantage of the fact that once the array is sorted, the largest gap must exist between two consecutive elements.

**Algorithm**

1. Sort the array.
2. Initialize a variable `maxGap` to store the maximum difference found.
3. Iterate through the sorted array, calculating the difference between every pair of consecutive elements.
4. Update `maxGap` if the current pair difference is larger than the previously recorded maximum difference.

**Time Complexity**:  
- Sorting the array takes \(O(n \log n)\).
- Iterating through the array once gives a total time complexity of \(O(n \log n)\).

**Space Complexity**:  
- Sorting typically requires \(O(n)\) additional space for the sorting algorithm.

```go
func maximumGap(nums []int) int {
    // Base case when there are less than 2 elements
    if len(nums) < 2 {
        return 0
    }
    
    // Sort the array
    sort.Ints(nums)
    
    maxGap := 0
    
    // Iterate through sorted array to find the maximum gap
    for i := 1; i < len(nums); i++ {
        // Calculate the difference between consecutive elements
        diff := nums[i] - nums[i-1]
        // Update maxGap if we find a larger difference
        if diff > maxGap {
            maxGap = diff
        }
    }
    
    return maxGap
}
```

### Optimal Approach: Bucket Sort

**Intuition**

A more efficient approach is to use a bucket sort technique (or pigeonhole principle). The idea is to bucket the numbers to efficiently find the maximum gap by leveraging the minimum and maximum values of each bucket rather than sorting the entire array.

To successfully utilize this approach, consider that:
- The largest possible gap is between the minimum and maximum elements.
- By dividing the range from min to max into `n-1` buckets, at least one bucket must be empty for such a gap to exist.

**Algorithm**

1. Find the minimum and maximum values of the array.
2. Calculate the `bucketSize` using the formula: (max - min) / (n - 1). Handle the case when all elements are the same.
3. Initialize buckets to keep track of the minimum and maximum values inside each bucket.
4. Distribute the elements into the appropriate buckets considering the offset.
5. Calculate the maximum gap by examining the difference in successive bucket boundaries.

**Time Complexity**:  
- Building the buckets and calculating the maximum gap takes \(O(n)\).

**Space Complexity**:  
- Additional space for bucket structure requires \(O(n)\).

```go
func maximumGap(nums []int) int {
    if len(nums) < 2 {
        return 0
    }
    
    minVal := nums[0]
    maxVal := nums[0]
    
    // Find the minimum and maximum values in nums
    for _, num := range nums {
        if num < minVal {
            minVal = num
        }
        if num > maxVal {
            maxVal = num
        }
    }
    
    // Calculate the gap
    n := len(nums)
    gap := max(1, (maxVal-minVal)/(n-1))
    
    // Number of buckets
    bucketCount := (maxVal-minVal)/gap + 1
    
    // Set up buckets
    minBucket := make([]int, bucketCount)
    maxBucket := make([]int, bucketCount)
    
    // Initialize buckets
    for i := range minBucket {
        minBucket[i] = math.MaxInt32
        maxBucket[i] = math.MinInt32
    }
    
    // Place each number in a bucket
    for _, num := range nums {
        if num == minVal || num == maxVal {
            continue
        }
        idx := (num - minVal) / gap
        minBucket[idx] = min(num, minBucket[idx])
        maxBucket[idx] = max(num, maxBucket[idx])
    }
    
    // Calculate max gap
    maxGap := 0
    prevMax := minVal
    
    for i := 0; i < bucketCount; i++ {
        if minBucket[i] == math.MaxInt32 {
            continue
        }
        maxGap = max(maxGap, minBucket[i] - prevMax)
        prevMax = maxBucket[i]
    }
    
    // Final comparison with global max
    maxGap = max(maxGap, maxVal - prevMax)
    
    return maxGap
}

// Helper functions to find maximum or minimum of two numbers
func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

This optimal solution ensures we utilize the large range between numbers efficiently by only focusing on the inter-bucket maximum differences.

