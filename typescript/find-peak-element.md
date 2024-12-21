# [Leetcode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approaches
- [Linear Scan](#linear-scan)
- [Binary Search](#binary-search)

## Linear Scan

### Intuition:
The simplest way to solve this problem is to traverse the array and find any peak element, which is an element that is higher than its neighbors. This approach typically guarantees finding a peak in a single pass through the array.

### Solution:
In this approach, we traverse from the start to the end of the array and check if the current element is greater than the next one. If it is, then the current element is a peak.

### Code:
```typescript
function findPeakElementLinear(nums: number[]): number {
    // Edge case for single element
    if (nums.length === 1) return 0;

    for (let i = 0; i < nums.length - 1; i++) {
        // Check if current element is greater than the next one
        if (nums[i] > nums[i + 1]) {
            // Current element is a peak
            return i;
        }
    }
    // If no peak is found, the last element is a peak
    return nums.length - 1;
}
```

### Time Complexity:
- O(n), where n is the number of elements in the array, as we might check each element once.

### Space Complexity:
- O(1), since no additional space is used.

## Binary Search

### Intuition:
To improve the time complexity, we can use a binary search approach. The idea is to use the properties of peaks and the fact that the array can be split into subproblems that either contain a peak or can be deterministically reduced.

### Solution:
We perform a binary search:
1. Choose a middle element.
2. If the middle element is greater than its right neighbor, a peak lies on the left or at the middle, so we move the right boundary.
3. Otherwise, move the left boundary because the peak must lie on the right.

### Code:
```typescript
function findPeakElementBinary(nums: number[]): number {
    let left = 0;
    let right = nums.length - 1;

    while (left < right) {
        const mid = Math.floor((left + right) / 2);

        // Compare the mid element with its right neighbor
        if (nums[mid] > nums[mid + 1]) {
            // If mid is greater, we move right because a peak must be on the left
            right = mid;
        } else {
            // Move left boundary to mid + 1 because a peak must be on the right
            left = mid + 1;
        }
    }

    // When left == right, we have found a peak
    return left;
}
```

### Time Complexity:
- O(log n), as we are reducing the search space by half in each step.

### Space Complexity:
- O(1), since we're using constant space for variables. 

In practice, this binary search approach is more efficient than the linear scan, especially for large arrays, while both methods will correctly find a peak element.

