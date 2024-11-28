# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n) per query
// Space Complexity: O(1)
public class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public int sumRange(int left, int right) {
        int sum = 0;

        // Calculate the sum for the range [left, right]
        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }

        return sum;
    }
}
```

## Approach 2: Prefix Sum (Optimal)

### Solution
```java
// Time Complexity: O(1) per query, O(n) for initialization
// Space Complexity: O(n)
public class NumArray {
    private int[] prefixSums;

    public NumArray(int[] nums) {
        int n = nums.length;
        prefixSums = new int[n + 1]; // Prefix sum array

        // Compute prefix sums
        for (int i = 0; i < n; i++) {
            prefixSums[i + 1] = prefixSums[i] + nums[i];
        }
    }

    public int sumRange(int left, int right) {
        // Use the prefix sum array to calculate the range sum
        return prefixSums[right + 1] - prefixSums[left];
    }
}
```