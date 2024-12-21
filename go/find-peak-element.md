# [LeetCode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approaches:

1. [Linear Scan Approach](#linear-scan-approach)
2. [Binary Search Approach](#binary-search-approach)

### Linear Scan Approach

The simplest approach to solve this problem is to perform a linear scan over the array and check for a peak element. A peak element is an element that is strictly greater than its neighbors. In the case of the first and last element, we check only one neighbor.

#### Intuition:

1. Traverse the array from the beginning to the end.
2. For each element, check if it is greater than its neighbors. More specifically, for element `nums[i]`, check if it is greater than `nums[i-1]` and `nums[i+1]`.
3. If we find such an element, return its index.

#### Code:

```go
func findPeakElement(nums []int) int {
    // Handle edge cases where the array has only one or two elements
    if len(nums) == 1 {
        return 0
    }
    
    // Loop over the array
    for i := 0; i < len(nums); i++ {
        // Check if current element is the peak
        if (i == 0 || nums[i] > nums[i-1]) && 
           (i == len(nums)-1 || nums[i] > nums[i+1]) {
            return i
        }
    }
    return -1 // This line will never be reached
}
```

#### Time and Space Complexity:

- **Time Complexity**: O(n), where n is the number of elements in the array. We perform a single pass through the array.
- **Space Complexity**: O(1), as no additional data structures are used.

### Binary Search Approach

A more efficient approach involves using a modified binary search. This approach capitalizes on the fact that if the middle element is not a peak, then there must be a peak in one of its two halves.

#### Intuition:

1. Use a binary search to find a peak element.
2. If the middle element is greater than its next element, it implies that one peak must be on the left half. Otherwise, there is at least one peak on the right half.
3. Continue the search until a peak element is found.

#### Code:

```go
func findPeakElement(nums []int) int {
    left, right := 0, len(nums)-1
    
    for left < right {
        mid := left + (right-left)/2  // Find the middle index
        
        if nums[mid] > nums[mid+1] {
            // If mid is greater than mid+1, then peak is in left half
            right = mid
        } else {
            // If mid is less than or equal to mid+1, then peak is in right half
            left = mid + 1
        }
    }
    
    // Since while loop exits when left == right, both signify the peak index
    return left
}
```

#### Time and Space Complexity:

- **Time Complexity**: O(log n), where n is the number of elements in the array. The binary search reduces the search space by half each iteration.
- **Space Complexity**: O(1), as no additional data structures are used. 

This approach is optimal and preferred for larger inputs due to its logarithmic time complexity.

