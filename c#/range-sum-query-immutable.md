# [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sum Approach](#prefix-sum-approach)

### Brute Force Approach

**Intuition:**

The brute force method involves calculating the sum of elements whenever a query is made. This means iterating over the range defined by the query and summing up the elements. Though simple, this approach is not efficient, especially when there are multiple range queries, as each query takes linear time in the worst case.

**Steps:**
1. For each query, iterate from the start index `i` to the end index `j`.
2. Accumulate the sum of elements for this range.
3. Return the accumulated sum.

**Code:**
```csharp
public class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }
    
    public int SumRange(int i, int j) {
        int sum = 0;
        // Iterate from index i to j and sum up the elements
        for (int k = i; k <= j; k++) {
            sum += nums[k];
        }
        return sum;
    }
}
```

**Time Complexity:** O(n) per query, where n is the number of elements in the query range.

**Space Complexity:** O(1), no extra space is used apart from input storage.

### Prefix Sum Approach

**Intuition:**

To optimize the queries, we can use an auxiliary array to store prefix sums. The idea here is to store the cumulative sum of elements up to each index. With the prefix sum array, we can find the sum of any range [i, j] in constant time.

**Steps:**
1. Construct a prefix sum array such that each element at index `k` stores the sum of elements from index `0` to `k`.
2. For a range query [i, j], calculate the sum as `prefixSum[j+1] - prefixSum[i]`.

**Code:**
```csharp
public class NumArray {
    private int[] prefixSum;

    public NumArray(int[] nums) {
        // Initialize the prefixSum array with one extra space for easier calculations
        prefixSum = new int[nums.Length + 1];
        // Build prefixSum where prefixSum[i] is sum of nums[0] to nums[i-1]
        for (int i = 0; i < nums.Length; i++) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
    }
    
    public int SumRange(int i, int j) {
        // Calculate the sum for the range i to j using prefix sums
        return prefixSum[j + 1] - prefixSum[i];
    }
}
```

**Time Complexity:** O(1) per query, since we compute the range sum using the prefix sum array.

**Space Complexity:** O(n), where n is the number of elements in the nums array, to store the prefix sums.

This second approach is extremely efficient for repeated range sum queries, as it requires precomputation but allows each query to be answered in constant time.

