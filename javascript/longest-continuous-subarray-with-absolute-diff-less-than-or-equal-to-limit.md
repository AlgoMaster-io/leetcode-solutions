# [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Solutions

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Deque](#approach-2-sliding-window-with-deque)

### Approach 1: Brute Force

#### Intuition

The brute force approach involves checking all subarrays of the given array and finding the one with the longest length where the absolute difference between the maximum and minimum values in the subarray is less than or equal to the limit. This approach is straightforward but not efficient, as it requires checking all possible subarrays.

#### Steps:
1. Iterate over the array with two nested loops.
2. For each pair of indices `(i, j)`, calculate the minimum and maximum values in the subarray `nums[i] to nums[j]`.
3. Check if the absolute difference between these values is less than or equal to the limit.
4. If it is, update the maximum length found so far.

#### Code
```javascript
function longestSubarray(nums, limit) {
    let n = nums.length;
    let maxLength = 0;

    // Iterate over each starting point of the subarray
    for (let start = 0; start < n; start++) {
        let minVal = nums[start];
        let maxVal = nums[start];

        // Iterating over the end point of the subarray
        for (let end = start; end < n; end++) {
            // Update min and max values in the current subarray
            minVal = Math.min(minVal, nums[end]);
            maxVal = Math.max(maxVal, nums[end]);

            // Check if this subarray satisfies the condition
            if (maxVal - minVal <= limit) {
                // Update the maximum length of the subarray found
                maxLength = Math.max(maxLength, end - start + 1);
            } else {
                break;
            }
        }
    }
    return maxLength;
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^2), where n is the size of the input array, because for each starting point, we iterate over all possible end points.
- **Space Complexity**: O(1), as we are using a constant amount of additional space.

### Approach 2: Sliding Window with Deque

#### Intuition

A more optimal approach is to use a sliding window technique combined with two deques. The deques help in keeping track of the maximum and minimum values in the current window efficiently. We adjust the window size to ensure the maximum minus minimum value remains within the limit.

#### Steps:
1. Initialize two deques: one for maintaining maximum values and another for minimum values.
2. Maintain a window represented by two indices `left` and `right`.
3. Expand the window by moving `right` and update the deques appropriately to maintain their properties.
4. When the maximum and minimum difference exceeds the limit, move the `left` pointer to shrink the window until the difference is within the limit.
5. Keep track of the maximum length of the window that satisfies the condition.

#### Code
```javascript
function longestSubarray(nums, limit) {
    let maxDeque = [];
    let minDeque = [];
    let left = 0, right = 0;
    let maxLength = 0;

    while (right < nums.length) {
        // Maintain maxDeque for maximum values
        while (maxDeque.length > 0 && nums[maxDeque[maxDeque.length - 1]] < nums[right]) {
            maxDeque.pop();
        }
        maxDeque.push(right);

        // Maintain minDeque for minimum values
        while (minDeque.length > 0 && nums[minDeque[minDeque.length - 1]] > nums[right]) {
            minDeque.pop();
        }
        minDeque.push(right);

        // If the current window violates the condition, shrink it
        while (nums[maxDeque[0]] - nums[minDeque[0]] > limit) {
            left++; // Move the left pointer to shrink the window
            if (maxDeque[0] < left) {
                maxDeque.shift(); // Remove out of bound element from maxDeque
            }
            if (minDeque[0] < left) {
                minDeque.shift(); // Remove out of bound element from minDeque
            }
        }

        maxLength = Math.max(maxLength, right - left + 1); // Update the maximum length found
        right++; // Expand the window by moving right
    }

    return maxLength;
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the size of the input array. Each element is added and removed from the deques at most once.
- **Space Complexity**: O(n), because in the worst case, the deques can contain all elements of the array.

