# [Leetcode Problem 209: Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Sliding Window](#approach-2-sliding-window)

---

## Approach 1: Brute Force

**Intuition:**

The simplest way to solve this problem is to consider each possible subarray of the given array. For each subarray, calculate the sum and check if it is greater than or equal to the given target sum `s`. The length of such subarray should be noted, and at the end, we need the minimum of such lengths.

**Code:**

```java
public int minSubArrayLen(int s, int[] nums) {
    int minLength = Integer.MAX_VALUE;
    // Iterate starting point of subarray
    for (int start = 0; start < nums.length; start++) {
        int sum = 0;
        // Iterate ending point of subarray
        for (int end = start; end < nums.length; end++) {
            sum += nums[end];
            // Check if current subarray sum is at least s
            if (sum >= s) {
                // Update minLength if current length is smaller
                minLength = Math.min(minLength, end - start + 1);
                break; // Move to the next start point
            }
        }
    }
    return (minLength == Integer.MAX_VALUE) ? 0 : minLength;
}
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n^2)\), where \(n\) is the number of elements in the array. This is because for each starting index, we iterate over the remaining elements.
- **Space Complexity:** \(O(1)\), since we only use a constant amount of additional space.

---

## Approach 2: Sliding Window

**Intuition:**

A more optimal solution involves using a sliding window technique. The main idea is to maintain a window that contains a sum greater than or equal to `s`. We expand the window by moving the end pointer and keep shrinking it from the start as long as the desired sum condition is satisfied. This helps in reducing the subarray size while maintaining the sum constraint.

**Code:**

```java
public int minSubArrayLen(int s, int[] nums) {
    int minLength = Integer.MAX_VALUE;
    int start = 0, sum = 0;
    // Iterate through the array with the end pointer
    for (int end = 0; end < nums.length; end++) {
        sum += nums[end];
        // Shrink the window from the start as long as the sum is sufficiently large
        while (sum >= s) {
            minLength = Math.min(minLength, end - start + 1);
            sum -= nums[start++];
        }
    }
    return (minLength == Integer.MAX_VALUE) ? 0 : minLength;
}
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n)\), where \(n\) is the number of elements in the array. Each element is added and removed from the sum at most once, resulting in linear time complexity.
- **Space Complexity:** \(O(1)\), since we only use a constant amount of additional space. 

---

This problem demonstrates the transition from a brute force solution, which checks every possibility, to a more efficient sliding window approach that optimally finds the minimal subarray length in one pass.

