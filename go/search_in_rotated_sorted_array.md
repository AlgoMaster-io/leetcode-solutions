# 33. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approach 1: Linear Search

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func search(nums []int, target int) int {
    // Iterate through the array to find the target
    for i, num := range nums {
        if num == target {
            return i // Return the index if found
        }
    }
    return -1 // Return -1 if target is not found
}
```

## Approach 2: Binary Search

### Solution
go
```go
// Time Complexity: O(log n)
// Space Complexity: O(1)
func search(nums []int, target int) int {
    start, end := 0, len(nums)-1

    // Perform binary search
    for start <= end {
        mid := start + (end-start)/2

        // Check if the middle element is the target
        if nums[mid] == target {
            return mid
        }

        // Determine which part of the array is sorted
        if nums[start] <= nums[mid] {
            // Left part is sorted
            if nums[start] <= target && target < nums[mid] {
                end = mid - 1 // Narrow search to the left
            } else {
                start = mid + 1 // Narrow search to the right
            }
        } else {
            // Right part is sorted
            if nums[mid] < target && target <= nums[end] {
                start = mid + 1 // Narrow search to the right
            } else {
                end = mid - 1 // Narrow search to the left
            }
        }
    }

    return -1 // Target not found
}
```

## Approach 3: Optimized Binary Search with Early Exit

### Solution
go
```go
// Time Complexity: O(log n)
// Space Complexity: O(1)
func search(nums []int, target int) int {
    start, end := 0, len(nums)-1

    // Perform binary search
    for start <= end {
        mid := start + (end-start)/2

        // If the target is found, return its index
        if nums[mid] == target {
            return mid
        }

        // Determine which part of the array is sorted
        if nums[start] <= nums[mid] {
            // Left part is sorted
            if nums[start] <= target && target < nums[mid] {
                end = mid - 1 // Target lies in the left part
            } else {
                start = mid + 1 // Target lies in the right part
            }
        } else {
            // Right part is sorted
            if nums[mid] < target && target <= nums[end] {
                start = mid + 1 // Target lies in the right part
            } else {
                end = mid - 1 // Target lies in the left part
            }
        }
    }

    return -1 // Return -1 if target is not found
}
```

