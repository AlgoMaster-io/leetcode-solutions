# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def maxSubarraySumCircular(self, nums):
        n = len(nums)
        max_sum = float('-inf')

        # Iterate over all possible subarrays, including circular ones
        for i in range(n):
            current_sum = 0

            for j in range(n):
                current_sum += nums[(i + j) % n]
                max_sum = max(max_sum, current_sum)

        return max_sum
```

## Approach 2: Optimized with Kadane’s Algorithm

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxSubarraySumCircular(self, nums):
        total_sum = 0
        max_sum, current_max = nums[0], 0
        min_sum, current_min = nums[0], 0

        for num in nums:
            # Calculate maximum subarray sum using Kadane's algorithm
            current_max = max(num, current_max + num)
            max_sum = max(max_sum, current_max)

            # Calculate minimum subarray sum using Kadane's algorithm
            current_min = min(num, current_min + num)
            min_sum = min(min_sum, current_min)

            # Calculate total sum of the array
            total_sum += num

        # If all numbers are negative, return the maximum sum (non-circular)
        return max_sum if max_sum > 0 else max(max_sum, total_sum - min_sum)
```

