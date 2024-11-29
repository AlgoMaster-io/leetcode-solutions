# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n) per query
// Space Complexity: O(1)
type NumArray struct {
	nums []int
}

func Constructor(nums []int) NumArray {
	return NumArray{nums: nums}
}

func (this *NumArray) SumRange(left int, right int) int {
	sum := 0

	// Calculate the sum for the range [left, right]
	for i := left; i <= right; i++ {
		sum += this.nums[i]
	}

	return sum
}
```

## Approach 2: Prefix Sum (Optimal)

### Solution
go
```go
// Time Complexity: O(1) per query, O(n) for initialization
// Space Complexity: O(n)
type NumArray struct {
	prefixSums []int
}

func Constructor(nums []int) NumArray {
	n := len(nums)
	prefixSums := make([]int, n+1) // Prefix sum array

	// Compute prefix sums
	for i := 0; i < n; i++ {
		prefixSums[i+1] = prefixSums[i] + nums[i]
	}

	return NumArray{prefixSums: prefixSums}
}

func (this *NumArray) SumRange(left int, right int) int {
	// Use the prefix sum array to calculate the range sum
	return this.prefixSums[right+1] - this.prefixSums[left]
}
```

