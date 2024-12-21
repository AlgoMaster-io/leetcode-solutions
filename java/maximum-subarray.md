# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Kadane's Algorithm](#kadanes-algorithm)

---

### Brute Force Approach

The brute force approach involves checking every possible subarray and computing the sum of each one. This method is straightforward but inefficient for larger arrays due to its high time complexity.

#### Intuition

1. Iterate through each element of the array, treating it as the start of a subarray.
2. For each start point, explore all possible end points and compute the sum for each subarray.
3. Track the maximum sum encountered.

#### Code

```java
public class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE; // Initialize to the smallest integer value
        
        // Iterate over each element treating it as the start of a subarray
        for (int i = 0; i < nums.length; i++) {
            int currentSum = 0;
            // Explore subarrays ending at each subsequent element
            for (int j = i; j < nums.length; j++) {
                currentSum += nums[j]; // Add the current element to the current subarray sum
                if (currentSum > maxSum) {
                    maxSum = currentSum; // Update maxSum if current subarray sum is greater
                }
            }
        }
        return maxSum;
    }
}
```

#### Time Complexity
- **O(n^2)**: We have a nested loop where for each element, we compute sums up to the end of the array.

#### Space Complexity
- **O(1)**: We only use a constant amount of extra space for variables.

---

### Kadane's Algorithm

Kadane's Algorithm provides an efficient way of finding the maximum subarray sum in linear time.

#### Intuition

1. Traverse the array, maintaining a running maximum sum of contiguous elements (`currentSum`).
2. If `currentSum` drops below zero, reset it to the current element, as a negative `currentSum` would decrease the sum of any subarray including it.
3. Track a global maximum (`maxSum`) across all encountered `currentSum` values.

#### Code

```java
public class Solution {
    public int maxSubArray(int[] nums) {
        int currentSum = nums[0]; // Start with the first element
        int maxSum = nums[0];     // Initialize maxSum with the first element

        // Traverse the array from the second element
        for (int i = 1; i < nums.length; i++) {
            // If currentSum is negative, reset to current element
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            // Update maxSum if currentSum is greater
            maxSum = Math.max(maxSum, currentSum);
        }
        return maxSum;
    }
}
```

#### Time Complexity
- **O(n)**: We traverse the array once, updating the running sum and maximum.

#### Space Complexity
- **O(1)**: We use a constant amount of extra space for variables (`currentSum` and `maxSum`).

---

Using these approaches, we can efficiently determine the maximum subarray sum for any integer array input. The brute force approach provides a clear understanding of the problem, while Kadane's Algorithm optimizes the solution to run in linear time.

