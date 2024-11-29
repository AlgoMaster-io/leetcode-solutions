# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approach 1: Using Built-in Sorting (Simplest)

### Solution
go
```go
// Time Complexity: O((m + n) log(m + n))
// Space Complexity: O(1)
import (
	"sort"
)

func merge(nums1 []int, m int, nums2 []int, n int) {
    // Copy nums2 into nums1
    copy(nums1[m:], nums2[:n])

    // Sort the combined array
    sort.Ints(nums1)
}
```

## Approach 2: Two Pointers with Temporary Array

### Solution
go
```go
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
func merge(nums1 []int, m int, nums2 []int, n int) {
    temp := make([]int, m+n)
    p1, p2, p := 0, 0, 0

    // Merge elements from nums1 and nums2 into temp
    for p1 < m && p2 < n {
        if nums1[p1] <= nums2[p2] {
            temp[p] = nums1[p1]
            p1++
        } else {
            temp[p] = nums2[p2]
            p2++
        }
        p++
    }

    // Copy remaining elements from nums1
    for p1 < m {
        temp[p] = nums1[p1]
        p1++
        p++
    }

    // Copy remaining elements from nums2
    for p2 < n {
        temp[p] = nums2[p2]
        p2++
        p++
    }

    // Copy temp back to nums1
    copy(nums1, temp)
}
```

## Approach 3: Two Pointers from End (Optimal)

### Solution
go
```go
// Time Complexity: O(m + n)
// Space Complexity: O(1)
func merge(nums1 []int, m int, nums2 []int, n int) {
    p1 := m - 1 // Pointer for the end of nums1's initial values
    p2 := n - 1 // Pointer for the end of nums2
    p := m + n - 1 // Pointer for the end of nums1's total capacity

    // Start merging from the back
    for p1 >= 0 && p2 >= 0 {
        if nums1[p1] > nums2[p2] {
            nums1[p] = nums1[p1]
            p1--
        } else {
            nums1[p] = nums2[p2]
            p2--
        }
        p--
    }

    // If there are leftover elements in nums2
    for p2 >= 0 {
        nums1[p] = nums2[p2]
        p2--
        p--
    }
}
```

