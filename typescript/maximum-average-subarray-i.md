# [643: Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approaches
- [Brute Force](#brute-force)
- [Sliding Window](#sliding-window)

---

### Brute Force

The brute force approach involves calculating all possible subarrays of size `k` and then finding their averages. This is a straightforward approach that checks all subarray possibilities but is not optimal in terms of performance.

#### Intuition
We loop through each possible starting index of a subarray of size `k`, sum the elements in the current subarray, calculate the average, and update the maximum average found so far.

#### Time Complexity
- **Time**: O(n*k), where `n` is the number of elements in the array and `k` is the subarray size. We calculate the sum for each subarray independently.
- **Space**: O(1), no extra space is used.

```typescript
function findMaxAverage(nums: number[], k: number): number {
    // Initialize the maximum average to the minimum possible value
    let maxAverage = -Infinity;

    // Iterate through each possible starting point of the subarray of size k
    for (let i = 0; i <= nums.length - k; i++) {
        let sum = 0;

        // Sum the elements of the subarray starting at index i
        for (let j = i; j < i + k; j++) {
            sum += nums[j];
        }

        // Calculate the average of this subarray and update the maximum average found
        const average = sum / k;
        maxAverage = Math.max(maxAverage, average);
    }

    // Return the highest average found
    return maxAverage;
}
```

---

### Sliding Window

The sliding window approach optimizes the brute force method by maintaining a running sum of the subarray and sliding the window across the array. This eliminates the need to recompute the sum from scratch for each subarray, leading to improved time complexity.

#### Intuition
- Start with the sum of the first `k` elements.
- Slide the window one step by subtracting the element going out of the window and adding the new element coming into the window.
- Update the maximum average during each step.

#### Time Complexity
- **Time**: O(n), we traverse the list once with stepwise adjustments.
- **Space**: O(1), additional space usage is constant.

```typescript
function findMaxAverage(nums: number[], k: number): number {
    let currentSum = 0;

    // Calculate the sum of the first k elements
    for (let i = 0; i < k; i++) {
        currentSum += nums[i];
    }

    // Initialize maxSum with the currentSum which is the sum of the first subarray of size k
    let maxSum = currentSum;

    // Slide the window from left to right over the array
    for (let i = k; i < nums.length; i++) {
        // Update the sum by subtracting the element that goes out of the window
        // and adding the element that comes into the window
        currentSum = currentSum - nums[i - k] + nums[i];

        // Update maxSum if the current window's sum is greater
        maxSum = Math.max(maxSum, currentSum);
    }

    // Calculate and return the maximum average
    return maxSum / k;
}
```

This sliding window approach efficiently finds the maximum average of a subarray of size `k` and is the recommended solution for this problem due to its optimal time complexity.

