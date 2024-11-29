# 34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approach 1: Linear Search

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func searchRange(nums []int, target int) []int {
    first, last := -1, -1

    // Traverse the array to find the first and last positions
    for i := 0; i < len(nums); i++ {
        if nums[i] == target {
            if first == -1 {
                first = i // Mark the first occurrence
            }
            last = i // Continuously update the last occurrence
        }
    }

    return []int{first, last}
}
```

## Approach 2: Binary Search (Two Separate Searches)

### Solution
go
```go
// Time Complexity: O(log n)
// Space Complexity: O(1)
package main

func searchRange(nums []int, target int) []int {
    first := findPosition(nums, target, true)  // Find first occurrence
    last := findPosition(nums, target, false) // Find last occurrence
    return []int{first, last}
}

func findPosition(nums []int, target int, findFirst bool) int {
    start, end, position := 0, len(nums)-1, -1

    for start <= end {
        mid := start + (end-start)/2

        if nums[mid] == target {
            position = mid // Record the position
            if findFirst {
                end = mid - 1 // Narrow search to the left
            } else {
                start = mid + 1 // Narrow search to the right
            }
        } else if nums[mid] < target {
            start = mid + 1 // Search in the right half
        } else {
            end = mid - 1 // Search in the left half
        }
    }

    return position
}
```

## Approach 3: Optimized Binary Search (Combined Search for Both Positions)

### Solution
go
```go
// Time Complexity: O(log n)
// Space Complexity: O(1)
package main

func searchRange(nums []int, target int) []int {
    result := []int{-1, -1}
    if len(nums) == 0 {
        return result // Return immediately if array is empty
    }

    // Find the first occurrence
    start, end := 0, len(nums)-1
    for start <= end {
        mid := start + (end-start)/2
        if nums[mid] < target {
            start = mid + 1 // Search in the right half
        } else {
            end = mid - 1 // Search in the left half
        }
        if nums[mid] == target {
            result[0] = mid // Update first position
        }
    }

    // Reset variables to find the last occurrence
    start, end = 0, len(nums)-1

    for start <= end {
        mid := start + (end-start)/2
        if nums[mid] <= target {
            start = mid + 1 // Search in the right half
        } else {
            end = mid - 1 // Search in the left half
        }
        if nums[mid] == target {
            result[1] = mid // Update last position
        }
    }

    return result
}
```


