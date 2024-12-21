# [Leetcode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches

- [Approach 1: Simple Kadane's Algorithm for Non-Circular Subarray](#approach-1-simple-kadanes-algorithm-for-non-circular-subarray)
- [Approach 2: Maximum Sum Circular Subarray Using Total and Minimum Sum](#approach-2-maximum-sum-circular-subarray-using-total-and-minimum-sum)

### Approach 1: Simple Kadane's Algorithm for Non-Circular Subarray

**Intuition:**

Kadane’s Algorithm is a classical approach used to find the largest sum contiguous subarray for standard, non-circular arrays. The idea is to iterate over the array while calculating the maximum sum ending at each position, updating the global maximum when a new higher sum is found.

Essentially, this approach handles the scenario where the maximum sum subarray does not wrap around.

**Steps:**

1. Initialize `currentMax` to the first element of the array, which tracks the maximum sum of subarrays ending at each position.
2. Initialize `globalMax` to the same value to keep track of the overall maximum sum found so far.
3. Traverse the array starting from the second element:
   - Calculate `currentMax` as the greater of the current element itself or the sum of `currentMax` and the current element.
   - Update `globalMax` whenever `currentMax` exceeds it.
4. Return `globalMax`, which represents the maximum sum contiguous subarray for a non-circular array.

```typescript
function kadane(nums: number[]): number {
    let currentMax = nums[0];
    let globalMax = nums[0];

    for (let i = 1; i < nums.length; i++) {
        // Determine if extending the subarray or starting a new one gives a higher sum
        currentMax = Math.max(nums[i], currentMax + nums[i]);
        // Update globalMax if currentMax is greater
        globalMax = Math.max(globalMax, currentMax);
    }
    return globalMax;
}

// Time Complexity: O(n) - We traverse the array once.
// Space Complexity: O(1) - Only a constant amount of extra space is used.
```

### Approach 2: Maximum Sum Circular Subarray Using Total and Minimum Sum

**Intuition:**

For a circular array, the maximum subarray sum can either be obtained without wrapping (handled by Kadane’s algorithm) or with wrapping. The wrapped subarray sum can be calculated through the total array sum minus the minimum subarray sum.

This approach combines finding both the maximum and minimum subarray sums to consider wrapping scenarios as well.

**Steps:**

1. Use Kadane’s algorithm to find the maximum subarray sum for the non-circular array.
2. Calculate the total sum of the array.
3. Use a modified Kadane's algorithm to find the minimum subarray sum, which enables us to determine the maximum wrap sum by subtracting this from the total.
4. Return the maximum of the maximum non-circular subarray sum and the calculated maximum wrap sum. However, if the maximum non-circular subarray sum is negative (resulting from all negative numbers), we stick with the non-circular result.

```typescript
function maxSubarraySumCircular(nums: number[]): number {
    // Step 1: Find the maximum subarray using Kadane's algorithm
    const maxSubarraySum = kadane(nums);

    // Step 2: Calculate total sum of the array
    const totalSum = nums.reduce((a, b) => a + b, 0);
    
    // Step 3: Find the minimum subarray sum using a modified Kadane's algorithm
    let currentMin = nums[0];
    let globalMin = nums[0];

    for (let i = 1; i < nums.length; i++) {
        currentMin = Math.min(nums[i], currentMin + nums[i]);
        globalMin = Math.min(globalMin, currentMin);
    }

    // Step 4: Calculate the potential maximum circular subarray sum
    const maxWrapSum = totalSum - globalMin;

    // If all numbers are negative, maxWrapSum will be zero, 
    // and maxSubarraySum will be the maximum single element (highest negative)
    return maxSubarraySum > 0 ? Math.max(maxSubarraySum, maxWrapSum) : maxSubarraySum;
}

// Time Complexity: O(n) - We make linear passes for Kadane and its modifications.
// Space Complexity: O(1) - Only a constant amount of extra space is used.
```

In the second approach, we account for both standard and circular subarrays by utilizing Kadane's algorithm twice — first for a maximum subarray and then for a minimum subarray, combining with the total to handle circularity elegantly. This method ensures that all edge cases, such as a completely negative array, are properly handled by comparing non-wrap and wrap scenarios.

