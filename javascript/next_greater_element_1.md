# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
javascript
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(1)
var nextGreaterElement = function(nums1, nums2) {
    let result = new Array(nums1.length);

    // Iterate over each element in nums1
    for (let i = 0; i < nums1.length; i++) {
        let current = nums1[i];
        let indexInNums2 = -1;

        // Find the index of current in nums2
        for (let j = 0; j < nums2.length; j++) {
            if (nums2[j] === current) {
                indexInNums2 = j;
                break;
            }
        }

        // Look for the next greater element to the right in nums2
        result[i] = -1; // Default if no greater element is found
        for (let j = indexInNums2 + 1; j < nums2.length; j++) {
            if (nums2[j] > current) {
                result[i] = nums2[j];
                break;
            }
        }
    }
    return result;
};
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
javascript
```javascript
// Time Complexity: O(n + m)
// Space Complexity: O(m)
var nextGreaterElement = function(nums1, nums2) {
    let nextGreaterMap = new Map();
    let stack = [];

    // Traverse nums2 in reverse to populate the nextGreaterMap
    for (let num of nums2) {
        // Maintain the stack in decreasing order
        while (stack.length > 0 && stack[stack.length - 1] <= num) {
            stack.pop();
        }
        // If stack is not empty, the top of the stack is the next greater element
        nextGreaterMap.set(num, stack.length === 0 ? -1 : stack[stack.length - 1]);
        stack.push(num);
    }

    // Build the result for nums1 using the map
    let result = new Array(nums1.length);
    for (let i = 0; i < nums1.length; i++) {
        result[i] = nextGreaterMap.get(nums1[i]);
    }

    return result;
};
```

