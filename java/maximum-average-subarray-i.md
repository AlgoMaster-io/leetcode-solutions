# [Leetcode 643: Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Optimized Sliding Window Approach](#optimized-sliding-window-approach)

---

### Brute Force Approach

#### Intuition:
The basic idea behind the brute force approach is to calculate the sum of every possible subarray of length `k` and then find the one with the maximum sum. This approach involves iterating over all the subarrays of length `k`, which can be computationally expensive for larger arrays.

#### Implementation:
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        // Initialize 'maxSum' to minimum possible value
        int maxSum = Integer.MIN_VALUE;
        
        // Iterate through each starting point of subarray of length k
        for (int i = 0; i <= nums.length - k; i++) {
            int currentSum = 0;
            
            // Calculate the sum of subarray starting at index 'i' of length 'k'
            for (int j = i; j < i + k; j++) {
                currentSum += nums[j];
            }
            
            // Update 'maxSum' if 'currentSum' is greater
            maxSum = Math.max(maxSum, currentSum);
        }
        
        // Return the maximum average
        return (double) maxSum / k;
    }
}
```

#### Time and Space Complexity:
- **Time Complexity**: O(n * k), where `n` is the length of `nums`. We calculate the sum for each possible subarray starting from each index.
- **Space Complexity**: O(1), only a fixed amount of extra space is used.

---

### Optimized Sliding Window Approach

#### Intuition:
The sliding window approach optimizes the calculation of the subarray sum by re-using the sum of the previous subarray. Rather than recalculating the sum from scratch for each subarray, we adjust the sum by subtracting the element that slides out of the window and adding the new element that comes into the window.

#### Implementation:
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int currentSum = 0;
        
        // Calculate sum of the first 'k' elements
        for (int i = 0; i < k; i++) {
            currentSum += nums[i];
        }
        
        int maxSum = currentSum;
        
        // Slide the window over the rest of the elements
        for (int i = k; i < nums.length; i++) {
            // Update the current sum by sliding the window
            currentSum = currentSum - nums[i - k] + nums[i];
            
            // Update maxSum if the current sum is greater
            maxSum = Math.max(maxSum, currentSum);
        }
        
        // Return the maximum average
        return (double) maxSum / k;
    }
}
```

#### Time and Space Complexity:
- **Time Complexity**: O(n), where `n` is the length of `nums`. We traverse the array once.
- **Space Complexity**: O(1), as we are using a constant amount of extra space. 

This sliding window approach is much more efficient for large arrays compared to the brute force approach.

