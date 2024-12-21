# [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Prefix Sum Approach](#prefix-sum-approach)

### Brute Force Approach

**Intuition:**

The brute force approach involves simply iterating through the subarray from index `i` to `j` each time a sum is requested. This is straightforward and easy to implement, but it's not efficient, especially for multiple queries.

**Algorithm:**

1. For each call to sumRange(i, j), iterate from index `i` to `j` in the nums array.
2. Accumulate the sum of numbers within this range.
3. Return the result.

```go
type NumArray struct {
    nums []int
}

func Constructor(nums []int) NumArray {
    return NumArray{nums}
}

func (this *NumArray) SumRange(i int, j int) int {
    sum := 0
    // Iterate over the desired range and sum up the elements
    for k := i; k <= j; k++ {
        // Accumulate the sum of the elements in the range
        sum += this.nums[k]
    }
    return sum
}
```

**Time Complexity:** O(n) per query, where n is the length of the subarray from `i` to `j`.

**Space Complexity:** O(1)

### Prefix Sum Approach

**Intuition:**

To improve the time complexity, we can preprocess the input array to generate a prefix sum array, where each element at index `i` of the prefix sum array contains the sum of elements from `0` to `i` in the original array. Using this prefix sum, any subarray sum can be computed in constant time.

**Algorithm:**

1. Preprocess the input array to create a `prefixSum` array.
2. `prefixSum[i]` contains the sum of elements from the start of the array up to index `i`.
3. For a query sumRange(i, j), the sum can be computed as `prefixSum[j] - prefixSum[i-1]`.
4. Handle the base case when `i` is 0 directly as `prefixSum[j]`.

```go
type NumArray struct {
    prefixSum []int
}

func Constructor(nums []int) NumArray {
    // Initialize the prefixSum array with an extra leading 0 to handle edge cases
    prefixSum := make([]int, len(nums)+1)
    // Compute the prefix sum array
    for i := 0; i < len(nums); i++ {
        // prefixSum[i+1] should store sum of all elements from nums[0] to nums[i]
        prefixSum[i+1] = prefixSum[i] + nums[i]
    }
    return NumArray{prefixSum}
}

func (this *NumArray) SumRange(i int, j int) int {
    // Calculate the sum of elements from i to j using the prefix sum array
    return this.prefixSum[j+1] - this.prefixSum[i]
}
```

**Time Complexity:** O(1) per query after an O(n) preprocessing step.

**Space Complexity:** O(n) due to the storage required for the `prefixSum` array.

