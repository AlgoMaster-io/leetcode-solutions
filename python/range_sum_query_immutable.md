# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n) per query
# Space Complexity: O(1)
class NumArray:
    def __init__(self, nums):
        self.nums = nums

    def sumRange(self, left, right):
        sum = 0

        # Calculate the sum for the range [left, right]
        for i in range(left, right + 1):
            sum += self.nums[i]

        return sum
```

## Approach 2: Prefix Sum (Optimal)

### Solution
```python
# Time Complexity: O(1) per query, O(n) for initialization
# Space Complexity: O(n)
class NumArray:
    def __init__(self, nums):
        n = len(nums)
        self.prefixSums = [0] * (n + 1)  # Prefix sum array

        # Compute prefix sums
        for i in range(n):
            self.prefixSums[i + 1] = self.prefixSums[i] + nums[i]

    def sumRange(self, left, right):
        # Use the prefix sum array to calculate the range sum
        return self.prefixSums[right + 1] - self.prefixSums[left]
```

