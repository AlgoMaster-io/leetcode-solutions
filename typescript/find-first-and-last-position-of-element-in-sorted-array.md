# [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approaches
- [Approach 1: Linear Scan](#approach-1-linear-scan)
- [Approach 2: Binary Search](#approach-2-binary-search)

## Approach 1: Linear Scan

### Intuition
The simplest way to solve this problem is to perform a linear scan through the array. We iterate through the array once to find the first occurrence of the target and again to find the last occurrence.

### Code
```typescript
function searchRange(nums: number[], target: number): number[] {
    let first = -1;
    let last = -1;
    
    // Traverse the entire array
    for (let i = 0; i < nums.length; i++) {
        // Update first occurrence
        if (nums[i] === target && first === -1) {
            first = i;
        }
        
        // Update last occurrence
        if (nums[i] === target) {
            last = i;
        }
    }
    
    return [first, last];
}
```

### Time Complexity
- O(n), where n is the number of elements in the array. We might scan the entire array in the worst case.

### Space Complexity
- O(1), as we are using a constant amount of extra space.

## Approach 2: Binary Search

### Intuition
Given that the array is sorted, a more efficient solution is to use binary search. We can perform a binary search twice: once to find the first position of the target and once to find the last position.

### Code
```typescript
function searchRange(nums: number[], target: number): number[] {
    // Helper function to find the first position
    const findFirst = (nums: number[], target: number): number => {
        let left = 0;
        let right = nums.length - 1;
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            // If target is found, move to the left to check for first occurrence
            if (nums[mid] === target) {
                if (mid === 0 || nums[mid - 1] !== target) {
                    return mid;
                }
                right = mid - 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    };

    // Helper function to find the last position
    const findLast = (nums: number[], target: number): number => {
        let left = 0;
        let right = nums.length - 1;
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            // If target is found, move to the right to check for last occurrence
            if (nums[mid] === target) {
                if (mid === nums.length - 1 || nums[mid + 1] !== target) {
                    return mid;
                }
                left = mid + 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    };

    const first = findFirst(nums, target);
    const last = findLast(nums, target);
    
    return [first, last];
}
```

### Time Complexity
- O(log n), where n is the number of elements in the array. We perform binary searches, each taking logarithmic time.

### Space Complexity
- O(1), as we are using a constant amount of extra space.

