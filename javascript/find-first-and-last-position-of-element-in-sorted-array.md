# [Leetcode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Table of Approaches
- [Approach 1: Linear Scan](#approach-1-linear-scan)
- [Approach 2: Binary Search](#approach-2-binary-search)
- [Approach 3: Optimized Binary Search](#approach-3-optimized-binary-search)

### Approach 1: Linear Scan

#### Intuition
A straightforward approach is to linearly scan through the array to find the first and last positions of the target element. We can iterate from the start to the end of the array and note down the first occurrence of the target. Similarly, iterate from the end to the start of the array to find the last occurrence.

#### Algorithm
1. Initialize `start` and `end` to -1 to signify the not found case.
2. Traverse the array from left to right for the first occurrence.
3. Traverse the array from right to left for the last occurrence.
4. Return the indices of the first and last occurrences.

```javascript
var searchRange = function(nums, target) {
    let start = -1; 
    let end = -1;

    // Finding the first occurrence
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === target) {
            start = i;
            break;
        }
    }

    // Finding the last occurrence
    for (let i = nums.length - 1; i >= 0; i--) {
        if (nums[i] === target) {
            end = i;
            break;
        }
    }

    return [start, end];
};
```

#### Complexity Analysis
- **Time Complexity**: \(O(n)\). We traverse the array twice.
- **Space Complexity**: \(O(1)\). No additional space is used.

### Approach 2: Binary Search

#### Intuition
Binary search can be utilized to find the position of the target in a sorted array. We would need two binary searches: one for finding the first occurrence and another for finding the last occurrence. This takes advantage of the sorted nature of the array.

#### Algorithm
1. Conduct a binary search to find the first occurrence of the target.
2. Conduct another binary search to find the last occurrence.
3. Return the indices of the first and last occurrences found.

```javascript
var searchRange = function(nums, target) {
    // Helper function to find the boundary
    const findBoundary = (nums, target, findFirst) => {
        let left = 0;
        let right = nums.length - 1;
        let boundaryIndex = -1;

        while (left <= right) {
            let mid = Math.floor((left + right) / 2);

            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else { // nums[mid] === target
                boundaryIndex = mid; // candidate found
                if (findFirst) {
                    right = mid - 1; // look on the left side
                } else {
                    left = mid + 1; // look on the right side
                }
            }
        }
        return boundaryIndex;
    };

    // Find the first and last positions
    let firstPosition = findBoundary(nums, target, true);
    let lastPosition = findBoundary(nums, target, false);

    return [firstPosition, lastPosition];
};
```

#### Complexity Analysis
- **Time Complexity**: \(O(\log n)\). Binary search halves the array size.
- **Space Complexity**: \(O(1)\). No additional space is used.

### Approach 3: Optimized Binary Search

#### Intuition
In the optimized binary search approach, we reduce the repeated boundary search by aligning the search into one single pass for both first and last positions. This is done by toggling the direction of binary search after finding a target.

#### Algorithm
1. Use a single binary search to find both first and last positions by toggling the direction once the target is found.
2. Record the positions efficiently without iterating over the array multiple times.
3. This saves overhead by managing both first and last occurrences within a single loop with conditions.

```javascript
var searchRange = function(nums, target) {
    let result = [-1, -1];

    // Search for the left boundary of the target range
    let left = 0;
    let right = nums.length - 1;

    // Find the first index
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    if (left < nums.length && nums[left] === target) {
        result[0] = left;
    } else {
        return result;
    }

    // Reset right boundary to find the last index
    right = nums.length - 1;

    // Find the last index
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        if (nums[mid] > target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }

    result[1] = right;
    
    return result;
};
```

#### Complexity Analysis
- **Time Complexity**: \(O(\log n)\). Each pass of binary search takes logarithmic time.
- **Space Complexity**: \(O(1)\). No additional memory is used.

