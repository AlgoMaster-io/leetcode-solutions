# [Leetcode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches
- [Basic Approach: Brute Force](#basic-approach-brute-force)
- [Kadane's Algorithm for Non-Circular Case](#kadane-algorithm-for-non-circular-case)
- [Optimal Approach: Using Modified Kadane's Algorithm](#optimal-approach-using-modified-kadanes-algorithm)

### Basic Approach: Brute Force

The most straightforward approach is to consider all possible subarrays, calculate their sums, and find the maximum sum. However, this approach would be inefficient due to its high time complexity.

#### Intuition:
- Consider every subarray, including wrapping subarrays by simulating a circular array.
- Calculate the sum of each subarray and keep track of the maximum sum found.

#### Time Complexity:
- O(N^2), where N is the number of elements in the array.

#### Space Complexity:
- O(1), since we're using a constant amount of extra space.

```javascript
function maxSubarraySumCircular(nums) {
    let n = nums.length;
    let maxSum = Number.NEGATIVE_INFINITY;

    // Loop over each possible starting point
    for (let start = 0; start < n; start++) {
        let currentSum = 0;

        // Loop over each possible array length
        for (let i = 0; i < n; i++) {
            currentSum += nums[(start + i) % n];  // Use modulo for circular wrapping
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Kadane's Algorithm for Non-Circular Case

Before tackling the circular nature of the problem, we first consider using Kadane's algorithm to solve the problem for a non-circular array, which can be used as part of the optimal solution.

#### Intuition:
- Kadane's algorithm helps find the maximum sum subarray in a linear array.
- It is based on the idea of maintaining a running sum of positive integers.
  
#### Time Complexity:
- O(N), where N is the number of elements in the array.

#### Space Complexity:
- O(1), as it only uses a constant amount of extra space.

```javascript
function kadane(nums) {
    let max_ending_here = nums[0];
    let max_so_far = nums[0];

    for (let i = 1; i < nums.length; i++) {
        max_ending_here = Math.max(nums[i], max_ending_here + nums[i]);
        max_so_far = Math.max(max_so_far, max_ending_here);
    }

    return max_so_far;
}

function maxSubarraySumCircular(nums) {
    return kadane(nums); // This only considers the non-circular case
}
```

### Optimal Approach: Using Modified Kadane's Algorithm

To account for the circular nature, the optimal solution involves using Kadane's algorithm twice: once for the standard situation and once for the complementary part of the circular array.

#### Intuition:
- Find the maximum sum of the subarray that does not wrap around using Kadane’s algorithm.
- Calculate the total array sum and use it to find the maximum sum if the subarray wraps around.
  - The wrapped maximum can be calculated by subtracting the minimum subarray sum (found using a modified Kadane’s) from the total array sum.
- The result will be the maximum of these two results unless all numbers are negative (in which case, we should return the maximum element).

#### Time Complexity:
- O(N), where N is the number of elements in the array.

#### Space Complexity:
- O(1), as it only requires a constant amount of additional space.

```javascript
function maxSubarraySumCircular(nums) {
    let totalSum = 0;
    let maxSumKadanes = Number.NEGATIVE_INFINITY;
    let currentMax = 0;
    let minSumKadanes = Number.POSITIVE_INFINITY;
    let currentMin = 0;

    for (let num of nums) {
        // Run Kadane's to find maximum subarray sum
        currentMax = Math.max(num, currentMax + num);
        maxSumKadanes = Math.max(maxSumKadanes, currentMax);

        // Run modified Kadane's to find minimum subarray sum
        currentMin = Math.min(num, currentMin + num);
        minSumKadanes = Math.min(minSumKadanes, currentMin);

        totalSum += num; // Sum all elements
    }
    
    // Handle the edge case of all negative numbers
    if (maxSumKadanes < 0) {
        return maxSumKadanes;
    }

    // Candidates for maximum circular sum are:
    // 1. Max contiguous sum found using standard Kadane's
    // 2. Total sum minus the sum of the minimum contiguous subarray for circular case
    return Math.max(maxSumKadanes, totalSum - minSumKadanes);
}
```

By handling circularity in this way, we effectively reduce the problem to a standard use of Kadane's algorithm with additional considerations for circular wrapping.

