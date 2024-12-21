# [LeetCode 4: Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approaches
- [Approach 1: Merging Two Arrays and Finding Median](#approach-1-merging-two-arrays-and-finding-median)
- [Approach 2: Binary Search (Optimal)](#approach-2-binary-search-optimal)

## Approach 1: Merging Two Arrays and Finding Median

### Intuition
The simplest approach to finding the median of two sorted arrays is to merge them into a single sorted array and then find the median. This method directly leverages the fact that we can create a sorted order from the merged arrays.

### Steps
1. Initialize two pointers, `i` and `j`, for `nums1` and `nums2` respectively.
2. Traverse both arrays and merge them into a single sorted array.
3. Compute the median from the merged array. If the length of the merged array is odd, the median is the middle element; if even, it's the average of the two middle elements.

### Time and Space Complexity
- **Time Complexity:** O(m + n), where m and n are the lengths of the input arrays. We traverse both arrays once.
- **Space Complexity:** O(m + n) for storing the merged array.

### Go Code

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    m, n := len(nums1), len(nums2)
    merged := make([]int, 0, m+n)
    i, j := 0, 0

    // Merge until one of the arrays is exhausted
    for i < m && j < n {
        if nums1[i] < nums2[j] {
            merged = append(merged, nums1[i])
            i++
        } else {
            merged = append(merged, nums2[j])
            j++
        }
    }
    
    // Append remaining elements of nums1, if any
    for i < m {
        merged = append(merged, nums1[i])
        i++
    }
    
    // Append remaining elements of nums2, if any
    for j < n {
        merged = append(merged, nums2[j])
        j++
    }

    totalLength := m + n
    if totalLength%2 == 0 {
        // Even length, average the two middle numbers
        return float64(merged[totalLength/2-1] + merged[totalLength/2]) / 2.0
    } else {
        // Odd length, return the middle number
        return float64(merged[totalLength/2])
    }
}
```

## Approach 2: Binary Search (Optimal)

### Intuition
Instead of merging the full arrays, we can utilize a binary search approach to effectively partition the arrays. By focusing on the smaller array, the binary search ensures a logarithmic complexity. This approach always seeks to find the perfect partition between the two arrays where all left elements are less than the right ones.

### Steps
1. Make sure `nums1` is the smaller array for reduced complexity.
2. Define low and high pointers for binary search, initializing them over the smaller array.
3. Adjust the partitions `pivot1` and `pivot2` using binary search, ensuring elements from left partitions are less than those from right partitions.
4. Determine the median based on partitioning indexes.

### Time and Space Complexity
- **Time Complexity:** O(log(min(m, n))), enabling efficient search through smaller array indices.
- **Space Complexity:** O(1), as no additional space is used, except for variables.

### Go Code

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    if len(nums1) > len(nums2) {
        return findMedianSortedArrays(nums2, nums1)
    }
    
    x, y := len(nums1), len(nums2)
    low, high := 0, x
    
    for low <= high {
        partitionX := (low + high) / 2
        partitionY := (x + y + 1) / 2 - partitionX
        
        maxX := math.MinInt64
        if partitionX != 0 {
            maxX = nums1[partitionX - 1]
        }
        
        maxY := math.MinInt64
        if partitionY != 0 {
            maxY = nums2[partitionY - 1]
        }
        
        minX := math.MaxInt64
        if partitionX != x {
            minX = nums1[partitionX]
        }
        
        minY := math.MaxInt64
        if partitionY != y {
            minY = nums2[partitionY]
        }
        
        if maxX <= minY && maxY <= minX {
            if (x + y) % 2 == 0 {
                return float64(max(maxX, maxY) + min(minX, minY)) / 2
            } else {
                return float64(max(maxX, maxY))
            }
        } else if maxX > minY {
            high = partitionX - 1
        } else {
            low = partitionX + 1
        }
    }
    
    panic("Input arrays are not sorted")
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

This binary search technique utilizes the concept of partitioning arrays such that all elements in the left partition are less than those in the right partition. This solution provides an efficient and optimal approach to resolve this problem.

