# 34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approach 1: Linear Search

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
var searchRange = function (nums, target) {
    let first = -1, last = -1;

    // Traverse the array to find the first and last positions
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === target) {
            if (first === -1) {
                first = i; // Mark the first occurrence
            }
            last = i; // Continuously update the last occurrence
        }
    }

    return [first, last];
};
```

## Approach 2: Binary Search (Two Separate Searches)

### Solution
```javascript
// Time Complexity: O(log n)
// Space Complexity: O(1)
var searchRange = function (nums, target) {
    const findPosition = (findFirst) => {
        let start = 0, end = nums.length - 1, position = -1;

        while (start <= end) {
            let mid = Math.floor(start + (end - start) / 2);

            if (nums[mid] === target) {
                position = mid; // Record the position
                if (findFirst) {
                    end = mid - 1; // Narrow search to the left
                } else {
                    start = mid + 1; // Narrow search to the right
                }
            } else if (nums[mid] < target) {
                start = mid + 1; // Search in the right half
            } else {
                end = mid - 1; // Search in the left half
            }
        }

        return position;
    };

    let first = findPosition(true);  // Find first occurrence
    let last = findPosition(false); // Find last occurrence
    return [first, last];
};
```

## Approach 3: Optimized Binary Search (Combined Search for Both Positions)

### Solution
```javascript
// Time Complexity: O(log n)
// Space Complexity: O(1)
var searchRange = function (nums, target) {
    let result = [-1, -1];
    if (nums.length === 0) {
        return result; // Return immediately if array is empty
    }

    // Find the first occurrence
    let start = 0, end = nums.length - 1;
    while (start <= end) {
        let mid = Math.floor(start + (end - start) / 2);
        if (nums[mid] < target) {
            start = mid + 1; // Search in the right half
        } else {
            end = mid - 1; // Search in the left half
        }
        if (nums[mid] === target) {
            result[0] = mid; // Update first position
        }
    }

    // Reset variables to find the last occurrence
    start = 0;
    end = nums.length - 1;

    while (start <= end) {
        let mid = Math.floor(start + (end - start) / 2);
        if (nums[mid] <= target) {
            start = mid + 1; // Search in the right half
        } else {
            end = mid - 1; // Search in the left half
        }
        if (nums[mid] === target) {
            result[1] = mid; // Update last position
        }
    }

    return result;
};
```

