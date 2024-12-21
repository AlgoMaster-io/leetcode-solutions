# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Table of Contents
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)

## Approach 1: Linear Search

### Intuition
The simplest way to find an element in an array is to iterate through every element until we find the target. While this method does not take advantage of the sorted and rotated nature of the array, it is a straightforward solution that guarantees finding the target if it's present.

### Solution
We iterate through the array from the start to the end. As soon as the target is matched with any element, we return its index. If we reach the end of the array without finding the target, we return -1.

### Time Complexity
- **Time:** O(n), where n is the number of elements in the array.
- **Space:** O(1), constant space is used.

### Code
```go
func search(nums []int, target int) int {
    // Iterate through the array
    for i := 0; i < len(nums); i++ {
        // If the target is found, return the index
        if nums[i] == target {
            return i
        }
    }
    // If the target is not found, return -1
    return -1
}
```

## Approach 2: Modified Binary Search

### Intuition
Since the original array was sorted and then rotated, it consists of two sorted sub-arrays. The key observation here is that at least one half of the array must be sorted. We can utilize this property to apply a binary search with modifications to find the target in O(log n) time.

### Solution
1. Begin with two pointers: `left` at the start and `right` at the end of the array.
2. Calculate the midpoint index `mid`.
3. Determine which part of the array is sorted:
   - If `nums[left] <= nums[mid]`, the left part is sorted.
   - Else, the right part is sorted.
4. Check if the target lies within the sorted half:
   - If it does, narrow down the pointers to that half.
   - If it doesn't, search the other half.
5. Repeat this process until `left` exceeds `right`.
6. If the target is found during the search, return its index. Otherwise, return -1.

### Time Complexity
- **Time:** O(log n), where n is the number of elements in the array, due to efficient binary search.
- **Space:** O(1), as no extra space is required beyond the input array and a few variables.

### Code
```go
func search(nums []int, target int) int {
    left, right := 0, len(nums)-1
    
    for left <= right {
        mid := left + (right-left)/2 // Calculate mid point to avoid overflow
        
        if nums[mid] == target {
            return mid // Found the target
        }
        
        // Determine the sorted part of the array
        if nums[left] <= nums[mid] {
            // Left half is sorted
            if nums[left] <= target && target < nums[mid] {
                right = mid - 1 // Target is in the left half
            } else {
                left = mid + 1 // Target is in the right half
            }
        } else {
            // Right half is sorted
            if nums[mid] < target && target <= nums[right] {
                left = mid + 1 // Target is in the right half
            } else {
                right = mid - 1 // Target is in the left half
            }
        }
    }
    
    return -1 // Target is not found
}
```

Each solution builds on understanding of the array's properties, ultimately leveraging rotations and sorting for efficient searching. The second approach showcases how analyzing array characteristics can help optimize search operations.

