# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches:
1. [Brute Force](#approach-1-brute-force)
2. [Optimized Approach using Kadane’s Algorithm](#approach-2-optimized-approach-using-kadanes-algorithm)

## Approach 1: Brute Force

### Intuition:
The brute force approach involves checking each subarray, calculating its sum, and keeping track of the maximum sum encountered. This approach directly evaluates all possible subarrays.

### Detailed Steps:
- Iterate through each element in the array, treating it as the starting point of a subarray.
- For each starting point, iterate through all possible ending points, creating subarrays.
- Calculate the sum of each subarray and compare it to the current maximum sum.
- Update the maximum sum if the current subarray's sum is greater.

### Code:
```javascript
function maxSubArray(nums) {
    // Initialize the maximum sum to the lowest possible integer.
    let maxSum = Number.MIN_SAFE_INTEGER;

    // Loop through each possible starting point of a subarray.
    for (let start = 0; start < nums.length; start++) {
        let currentSum = 0;

        // Loop through each possible ending point.
        for (let end = start; end < nums.length; end++) {
            // Add the current element to the subarray sum.
            currentSum += nums[end];

            // Update maxSum if we found a new maximum.
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    // Return the maximum sum found.
    return maxSum;
}
```

### Time Complexity:
- **O(n^2)**, where n is the number of elements in the array. Nested loops result in n*(n+1)/2 subarray sums being calculated.

### Space Complexity:
- **O(1)**, no extra space is used other than variables for tracking the maximum sum.

---

## Approach 2: Optimized Approach using Kadane’s Algorithm

### Intuition:
Kadane’s Algorithm is a dynamic programming approach that efficiently finds the maximum sum of a contiguous subarray. The key idea is to keep a running sum that considers the maximum at each point by evaluating whether to start a new subarray at the current element or continue the existing one.

### Detailed Steps:
- Initialize variables to track the current subarray sum and global maximum.
- Iterate through the array:
  - For each element, decide whether to add it to the existing subarray or start fresh if the current element itself is larger than the sum including it.
  - Update the global maximum if the current subarray's sum is greater.
- This way, at any point, the running sum represents the best possible subarray ending at that index.

### Code:
```javascript
function maxSubArray(nums) {
    // Initialize the running sum and the maximum sum as the first element.
    let currentSum = nums[0];
    let maxSum = nums[0];

    // Iterate through the array starting from the second element.
    for (let i = 1; i < nums.length; i++) {
        // Decide whether to add the current element to the existing subarray or start a new one.
        currentSum = Math.max(nums[i], currentSum + nums[i]);

        // Update the maximum subarray sum
        maxSum = Math.max(maxSum, currentSum);
    }

    // Return the maximum subarray sum found.
    return maxSum;
}
```

### Time Complexity:
- **O(n)**, as we traverse the array once, making decisions at each element.

### Space Complexity:
- **O(1)**, since no extra space is used other than variables for tracking sums. 

With Kadane’s Algorithm, the problem is solved in linear time, making it the most efficient approach for this problem.

