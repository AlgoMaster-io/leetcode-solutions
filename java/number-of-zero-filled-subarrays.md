# [Leetcode 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

In this problem, we are tasked with determining the number of contiguous subarrays in a given array that are entirely filled with zeroes. The array may contain positive and negative numbers as well.

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Linear Scan](#approach-2-optimized-linear-scan)

### Approach 1: Brute Force

**Intuition**:  
The simplest approach to solve this problem is to consider all possible subarrays and count those filled entirely with zeroes. While this method is straightforward, it can be inefficient for large arrays due to the sheer number of potential subarrays.

**Algorithm**:
1. Iterate over all possible starting points of subarrays.
2. For each starting point, extend the subarray endpoint till we are passing zeroes.
3. Count the subarrays when a complete run of zeroes is detected.

**Code**:
```java
public class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long count = 0;
        int n = nums.length;
        
        // Check each subarray starting at index `i`
        for (int i = 0; i < n; i++) {
            // Track contiguous zeroes starting from index `i`
            for (int j = i; j < n && nums[j] == 0; j++) {
                // Increment count each time a zero-filled subarray is found
                count++;
            }
        }
        
        return count;
    }
}
```

**Time Complexity**: O(n^2) - As we check each subarray which takes quadratic time.  
**Space Complexity**: O(1) - No additional space is used beyond input size.

### Approach 2: Optimized Linear Scan

**Intuition**:  
We can achieve a more optimal solution by recognizing runs of zeroes as we iterate over the array. For each segment of contiguous zeroes, the number of zero-filled subarrays can be calculated using combinatorial arithmetic. Specifically, a contiguous run of `k` zeroes contributes `k * (k + 1) / 2` zero-filled subarrays.

**Algorithm**:
1. Initialize a counter for zero segments (`zeroCount`) and the total result (`result`).
2. Traverse the array.
3. For every zero encountered, increase the `zeroCount`.
4. If a non-zero is encountered or the array ends, add the count of zero-filled subarrays for the segment, i.e., `zeroCount * (zeroCount + 1) / 2` to the result, and reset `zeroCount`.
5. Return the result.

**Code**:
```java
public class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long result = 0;
        int zeroCount = 0;
        
        for (int num : nums) {
            // Check if the current number is zero
            if (num == 0) {
                // Extend the current zero sequence
                zeroCount++;
            } else {
                // Calculate subarrays for the zero sequence
                result += zeroCount * (zeroCount + 1L) / 2;
                // Reset zero sequence counter
                zeroCount = 0;
            }
        }
        
        // If the last element was part of a zero sequence, add its subarrays
        result += zeroCount * (zeroCount + 1L) / 2;
        
        return result;
    }
}
```

**Time Complexity**: O(n) - We pass through the array a single time.  
**Space Complexity**: O(1) - Only a constant amount of extra space is used.

In conclusion, the optimized linear scan approach is significantly more efficient and elegant for this problem, particularly when dealing with large input sizes.

