# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches
1. [Brute Force](#brute-force)
2. [Divide and Conquer](#divide-and-conquer)
3. [Kadane's Algorithm](#kadanes-algorithm)

## Brute Force

The brute force approach involves considering all possible subarrays within the input array and calculating the sum of each to determine which one is the maximum.

### Intuition:
- Loop through each starting point of a subarray.
- For each starting point, loop through possible ending points.
- Calculate the sum of elements starting from the starting point to the ending point.
- Track the maximum sum encountered.

### Code:
```typescript
function maxSubArrayBruteForce(nums: number[]): number {
    let maxSum = Number.MIN_SAFE_INTEGER;

    // Start at each index as a potential starting point of subarray
    for (let i = 0; i < nums.length; i++) {
        let currentSum = 0;
        // Iterate to get the end index of the subarray
        for (let j = i; j < nums.length; j++) {
            // Add the current element to the currentSum
            currentSum += nums[j];
            // Update maxSum if we found a new max
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

### Complexity:
- **Time Complexity**: O(n^2), where n is the length of the input array. This is because there are n potential starting points, and for each starting point, we sum over remaining elements.
- **Space Complexity**: O(1), as we are using only a fixed amount of additional space.

## Divide and Conquer

This approach breaks the problem into subproblems and utilizes the recursive approach to find the maximum subarray sum.

### Intuition:
- Divide the array into two halves.
- Recursively find the maximum subarray sum for the left half, the right half, and the maximum subarray that crosses the midpoint.
- The solution to the original problem is the maximum of these three values.

### Code:
```typescript
function maxSubArrayDivideAndConquer(nums: number[]): number {
    function helper(left: number, right: number): number {
        if (left === right) {
            return nums[left];
        }

        // Find the midpoint of the current array section
        const mid = Math.floor((left + right) / 2);

        // Recursively find the max subarray sum in the left half, right half, and crossing the mid
        const leftMax = helper(left, mid);
        const rightMax = helper(mid + 1, right);
        const crossMax = findCrossMax(nums, left, mid, right);

        // Return the maximum of the three possible scenarios
        return Math.max(leftMax, rightMax, crossMax);
    }

    function findCrossMax(nums: number[], left: number, mid: number, right: number): number {
        let leftSum = Number.MIN_SAFE_INTEGER;
        let currentSum = 0;

        // Calculate max sum of subarray crossing the mid, from left to mid
        for (let i = mid; i >= left; i--) {
            currentSum += nums[i];
            leftSum = Math.max(leftSum, currentSum);
        }

        let rightSum = Number.MIN_SAFE_INTEGER;
        currentSum = 0;

        // Calculate max sum of subarray crossing the mid, from mid+1 to right
        for (let i = mid + 1; i <= right; i++) {
            currentSum += nums[i];
            rightSum = Math.max(rightSum, currentSum);
        }

        // Combine the maximum subarrays on each side of the mid
        return leftSum + rightSum;
    }

    return helper(0, nums.length - 1);
}
```

### Complexity:
- **Time Complexity**: O(n log n), as the recurrence relation is T(n) = 2T(n/2) + O(n).
- **Space Complexity**: O(log n), due to the recursive call stack depth.

## Kadane's Algorithm

Kadane's Algorithm is an efficient approach that finds the maximum subarray sum in linear time.

### Intuition:
- Iterate through the array while maintaining a running sum of the maximum subarray ending at the current position.
- If the running sum becomes negative, reset it.
- Track the overall maximum sum encountered during the iteration.

### Code:
```typescript
function maxSubArrayKadane(nums: number[]): number {
    let maxSum = nums[0];
    let currentSum = nums[0];

    // Start with the first element as the initial subarray sum
    for (let i = 1; i < nums.length; i++) {
        // Decide whether to add the current element to the existing subarray or start fresh
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        // Update maxSum if the currentSum is greater
        maxSum = Math.max(maxSum, currentSum);
    }

    return maxSum;
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the length of the input array, as we traverse the array once.
- **Space Complexity**: O(1), since no extra space is used other than for variables to store sums.

