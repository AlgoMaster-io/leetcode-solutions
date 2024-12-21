# [Leetcode Problem 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

### Approaches
1. [Linear Search Approach](#linear-search-approach)
2. [Binary Search Approach](#binary-search-approach)

---

## Linear Search Approach

### Intuition
The linear search approach is straightforward. We iterate through each element of the array and check if the current element is greater than or equal to the target. If found, it means that the target should be inserted at that index to maintain the sorted order. If the target is greater than all elements, it should be inserted at the end of the array. This approach is simple but not efficient for large arrays.

### Code
```go
func searchInsert(nums []int, target int) int {
    // Iterate over each element in the array
    for i, num := range nums {
        // If the current element is equal or greater than the target
        // return the current index
        if num >= target {
            return i
        }
    }
    // If we reach here, it means the target is greater than all elements
    // Return the length of the array, which is the index position where the target would be inserted
    return len(nums)
}
```

### Time Complexity
- **Time**: O(n), where n is the number of elements in the array, because we might have to iterate through all elements.
- **Space**: O(1), as we are not using any additional space except for variables.

---

## Binary Search Approach

### Intuition
The binary search technique takes advantage of the fact that the input array is sorted. By repeatedly dividing the array into half, we can effectively reduce the search space and find the index where the target should be inserted. This is more efficient compared to linear search as it significantly reduces the number of comparisons needed.

### Code
```go
func searchInsertBinary(nums []int, target int) int {
    left, right := 0, len(nums)-1

    // Continue searching while the left index does not surpass the right index
    for left <= right {
        // Find the middle index
        mid := left + (right-left)/2

        // Check if the mid element is equal to the target
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            // If middle is less than target, narrow the search to the right half
            left = mid + 1
        } else {
            // If middle is more than target, narrow the search to the left half
            right = mid - 1
        }
    }
    // If we exit the loop, left is the index where target should be inserted
    return left
}
```

### Time Complexity
- **Time**: O(log n), where n is the number of elements in the array, because we are halving the search space with each operation.
- **Space**: O(1), as we are only using a constant amount of extra space for variables.

---

Each of these methods provides a solution to the problem, with the binary search approach being significantly more optimal for larger data sets due to its logarithmic time complexity.

