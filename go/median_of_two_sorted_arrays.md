# 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach 1: Brute Force (Merge and Find Median)

### Solution

go
```go
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
package main

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	m, n := len(nums1), len(nums2)
	merged := make([]int, m+n)
	
	// Merge the two arrays into one sorted array
	i, j, k := 0, 0, 0
	for i < m && j < n {
		if nums1[i] < nums2[j] {
			merged[k] = nums1[i]
			i++
		} else {
			merged[k] = nums2[j]
			j++
		}
		k++
	}

	// Add remaining elements from nums1
	for i < m {
		merged[k] = nums1[i]
		i++
		k++
	}

	// Add remaining elements from nums2
	for j < n {
		merged[k] = nums2[j]
		j++
		k++
	}

	// Find and return the median
	totalLength := m + n
	if totalLength%2 == 0 {
		return float64(merged[totalLength/2-1]+merged[totalLength/2]) / 2.0
	} else {
		return float64(merged[totalLength/2])
	}
}
```

## Approach 2: Binary Search on Partition Index (Optimal Solution)

### Solution

go
```go
// Time Complexity: O(log(min(m, n)))
// Space Complexity: O(1)
package main

import (
	"math"
)

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	if len(nums1) > len(nums2) {
		// Ensure nums1 is the smaller array
		return findMedianSortedArrays(nums2, nums1)
	}

	m, n := len(nums1), len(nums2)
	start, end := 0, m

	for start <= end {
		partitionX := start + (end-start)/2
		partitionY := (m+n+1)/2 - partitionX

		// Edge cases: Use Int32.MinValue and Int32.MaxValue for out-of-bound partitions
		maxX := getMax(nums1, partitionX)
		minX := getMin(nums1, partitionX, m)
		maxY := getMax(nums2, partitionY)
		minY := getMin(nums2, partitionY, n)

		if maxX <= minY && maxY <= minX {
			// Correct partition found
			if (m+n)%2 == 0 {
				// Even total length: Average of max of left and min of right
				return (math.Max(float64(maxX), float64(maxY)) + math.Min(float64(minX), float64(minY))) / 2.0
			} else {
				// Odd total length: Max of left partition
				return math.Max(float64(maxX), float64(maxY))
			}
		} else if maxX > minY {
			// Move left in nums1
			end = partitionX - 1
		} else {
			// Move right in nums1
			start = partitionX + 1
		}
	}

	panic("Input arrays are not sorted or invalid.")
}

func getMax(nums []int, partition int) int {
	if partition == 0 {
		return math.MinInt32
	}
	return nums[partition-1]
}

func getMin(nums []int, partition int, length int) int {
	if partition == length {
		return math.MaxInt32
	}
	return nums[partition]
}
```

