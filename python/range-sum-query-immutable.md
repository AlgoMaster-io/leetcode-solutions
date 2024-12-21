# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approaches
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Prefix Sum Approach](#approach-2-prefix-sum-approach)

---

## Approach 1: Brute Force Approach

### Intuition
In the brute force approach, for each query, we calculate the sum of the elements between indices `i` and `j` by iterating through and accumulating the values. This approach is straightforward but inefficient for multiple queries as each query takes O(n) time.

### Code
```python
class NumArray:

    def __init__(self, nums):
        # Initialize with the array of numbers
        self.nums = nums

    def sumRange(self, i, j):
        # Calculate the sum from index i to j inclusive
        total = 0
        for index in range(i, j + 1):
            total += self.nums[index]
        return total
```

### Time Complexity
- **Constructor:** O(1) - Simply stores the reference.
- **sumRange:** O(n) per query, where n is the number of elements between i and j.

### Space Complexity
- O(1) since no extra space is used except the reference.

---

## Approach 2: Prefix Sum Approach

### Intuition
To optimize the sum calculation, we can use a prefix sum array. The prefix sum at each index `k` stores the sum of numbers from the start up to `k`. This allows us to calculate the sum for any range in constant time by subtracting the prefix sum values.

### Code
```python
class NumArray:

    def __init__(self, nums):
        # Calculate the prefix sums
        self.prefix_sums = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.prefix_sums[i + 1] = self.prefix_sums[i] + nums[i]

    def sumRange(self, i, j):
        # Using prefix sums to get the sum in O(1) time
        return self.prefix_sums[j + 1] - self.prefix_sums[i]
```

### Time Complexity
- **Constructor:** O(n), where n is the length of the array due to prefix sums calculation.
- **sumRange:** O(1) per query.

### Space Complexity
- O(n) for storing the prefix sums array of size n+1.

---

By using the prefix sum approach, we significantly optimize repeated queries for sum ranges, which makes it suitable for scenarios where multiple queries are required on a large data set.

