Here's a markdown documentation for solving the Leetcode problem: "Find Minimum in Rotated Sorted Array".

---

## [Leetcode Problem 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

### Approaches
1. [Linear Scan](#linear-scan)
2. [Binary Search](#binary-search)

---

### 1. Linear Scan

#### Intuition:
The simplest approach is to iterate through the array and find the minimum element. A rotated sorted array maintains the rule that the minimum element in the array is between any two numbers where the order is disrupted (i.e., a larger number is followed by a smaller number).

#### Implementation:
```javascript
var findMin = function(nums) {
    // Initialize the minimum with the first element
    let min = nums[0];
    
    // Iterate over the array to find the minimum element
    for (let i = 1; i < nums.length; i++) {
        // Update the minimum if a smaller element is found
        if (nums[i] < min) {
            min = nums[i];
        }
    }
    
    return min;
};
```

#### Complexity Analysis:
- **Time Complexity**: O(n) because we traverse the entire array once.
- **Space Complexity**: O(1) because we use a constant amount of space.

---

### 2. Binary Search

#### Intuition:
Since the array is a rotated version of a sorted array, it can be broken into two sorted subarrays. A binary search approach can help efficiently find the minimum by exploiting the fact that the smallest element is the only element whose previous is greater than it (or it is the first element in the array). Use binary search to minimize the number of comparisons by adjusting the search to increase the likelihood of finding the minimum with fewer checks.

#### Implementation:
```javascript
var findMin = function(nums) {
    let left = 0;
    let right = nums.length - 1;
    
    // When the array is not rotated (first element is the minimum)
    if (nums[left] < nums[right]) return nums[left];
    
    while (left < right) {
        // Find the mid index
        let mid = Math.floor((left + right) / 2);

        // Check if element at mid is the minimum
        if (mid > 0 && nums[mid] < nums[mid - 1]) return nums[mid];
        
        // Check which side is unsorted and adjust the boundary
        if (nums[mid] >= nums[left]) {
            // The left side is sorted, so the unsorted minimum is in the right
            left = mid + 1;
        } else {
            // The right side is sorted, so the unsorted minimum is in the left
            right = mid;
        }
    }
    
    return nums[left];
};
```

#### Complexity Analysis:
- **Time Complexity**: O(log n) because we halve the search space with each pass.
- **Space Complexity**: O(1) because we use a constant amount of space.

---

These approaches provide solutions from basic to optimized based on the constraints and properties of the input array. The binary search approach is preferred for its logarithmic time complexity, especially for large arrays.

