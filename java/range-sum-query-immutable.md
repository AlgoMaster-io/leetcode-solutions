# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Prefix Sum Array Approach](#approach-2-prefix-sum-array-approach)

---

### Approach 1: Brute Force Approach

The simplest way to solve this problem is to calculate the sum of elements from `i` to `j` every time a query is made.

**Intuition:**
- For each query, loop through the range from `i` to `j` and sum up the elements.
- This approach is straightforward but not efficient for large numbers of queries or larger input sizes.

```java
class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        // Store the original array
        this.nums = nums;
    }
    
    public int sumRange(int i, int j) {
        int sum = 0;
        // Loop through the range and calculate the sum
        for (int k = i; k <= j; k++) {
            sum += nums[k];
        }
        return sum;
    }
}
```

**Time Complexity:** `O(n)` per query, where `n` is the number of elements in the range `[i, j]`.

**Space Complexity:** `O(1)` for handling each query since no additional space is used other than input storage.

---

### Approach 2: Prefix Sum Array Approach

A more optimal approach is to preprocess the input array to create a prefix sum array.

**Intuition:**
- The prefix sum at each index `i` stores the cumulative sum of the array from `0` to `i`.
- For a query to get the sum from index `i` to `j`, we can use the formula:
  \[
  \text{sumRange}(i, j) = \text{prefixSum}[j] - \text{prefixSum}[i-1]
  \]
- This reduces the time complexity of each query to `O(1)`.

**Steps:**
1. Initialize a prefix sum array with an extra zero at the start for ease of calculation.
2. For each element in the input array, calculate the cumulative sum and store it in the prefix sum array.
3. When a query is made, use the formula above to get the sum in constant time.

```java
class NumArray {
    private int[] prefixSums;

    public NumArray(int[] nums) {
        // Initialize the prefix sums array
        prefixSums = new int[nums.length + 1];
        
        // Build the prefix sum array
        for (int i = 0; i < nums.length; i++) {
            prefixSums[i + 1] = prefixSums[i] + nums[i];
        }
    }
    
    public int sumRange(int i, int j) {
        // Use prefix sums to calculate the range sum in O(1) time
        return prefixSums[j + 1] - prefixSums[i];
    }
}
```

**Time Complexity:** 
- `O(n)` for preprocessing where `n` is the number of elements in the input array.
- `O(1)` for each sum range query.

**Space Complexity:** `O(n)` due to the prefix sum array.

This approach significantly optimizes the query time and is suitable for handling multiple queries efficiently.

