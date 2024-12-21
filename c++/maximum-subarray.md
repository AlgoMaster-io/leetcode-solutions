# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches:
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Kadane's Algorithm (Optimal)](#approach-2-kadanes-algorithm-optimal)

---

## Approach 1: Brute Force Approach

### Intuition:
The brute force approach simply involves considering all possible subarrays and calculating their sums. The maximum sum encountered during this process is the answer. While very straightforward, this approach is inefficient due to its high time complexity.

### Detailed Steps:
1. Initialize a variable `maxSum` with the smallest possible integer value to keep track of the maximum sum found.
2. Iterate through all possible starting points of subarrays.
3. For each starting point, iterate through all possible ending points.
4. Compute the sum of elements between the starting and ending point.
5. If the computed sum is greater than `maxSum`, update `maxSum`.
6. After finishing, `maxSum` will hold the maximum sum of subarrays.

### C++ Code:
```cpp
#include <vector>
#include <climits>

int maxSubArray(std::vector<int>& nums) {
    int maxSum = INT_MIN; // Initialize maxSum to the smallest integer
    int n = nums.size();  // Get size of the vector
    for (int i = 0; i < n; ++i) { // Iterate over the starting point
        int currentSum = 0; // Initialize currentSum for this starting point
        for (int j = i; j < n; ++j) { // Iterate over the ending point
            currentSum += nums[j]; // Add element to currentSum
            if (currentSum > maxSum) { // Update maxSum if currentSum is greater
                maxSum = currentSum;
            }
        }
    }
    return maxSum; // Return the found maximum sum
}
```

### Time and Space Complexity:
- **Time Complexity**: O(n^2), where n is the number of elements in the array. This is because we have a nested loop iterating through all possible subarrays.
- **Space Complexity**: O(1), as we are using a constant amount of space.

---

## Approach 2: Kadane's Algorithm (Optimal)

### Intuition:
Kadane's Algorithm improves on the brute force method by maintaining a running sum of the subarray and resetting it if it becomes negative. The key insight is that a negative sum cannot contribute to a maximum subarray, so restarting a potential subarray from the next element is optimal.

### Detailed Steps:
1. Initialize two variables: `maxSum` with the smallest possible integer and `currentSum` to zero.
2. Iterate through each element in the array.
3. Add the current element to `currentSum`.
4. If `currentSum` exceeds `maxSum`, update `maxSum`.
5. If `currentSum` becomes negative, reset it to zero, as this means starting any subarray past this point with `currentSum` is disadvantageous.
6. Once iteration completes, `maxSum` contains the maximum sum of a subarray.

### C++ Code:
```cpp
#include <vector>
#include <climits>

int maxSubArray(std::vector<int>& nums) {
    int maxSum = INT_MIN;  // Initialize maxSum to the smallest integer
    int currentSum = 0;    // Initialize current sum to 0
    for (int num : nums) { // Iterate over each element in the array
        currentSum += num; // Add the current element to currentSum
        if (currentSum > maxSum) { // Update maxSum if currentSum is greater
            maxSum = currentSum;
        }
        if (currentSum < 0) { // Reset currentSum if it becomes negative
            currentSum = 0;
        }
    }
    return maxSum; // Return the found maximum sum
}
```

### Time and Space Complexity:
- **Time Complexity**: O(n), where n is the number of elements in the array. This is due to a single pass through the array.
- **Space Complexity**: O(1), as we only use a finite number of variables regardless of the input size.

---

Each approach provides a valid method to solve the problem, with the first approach being more understandable but less efficient, and the second approach offering significant improvements in time complexity.

