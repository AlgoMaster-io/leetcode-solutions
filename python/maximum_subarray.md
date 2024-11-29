# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def maxSubArray(self, nums):
        max_sum = float('-inf')

        # Check all possible subarrays
        for i in range(len(nums)):
            current_sum = 0

            for j in range(i, len(nums)):
                current_sum += nums[j]
                max_sum = max(max_sum, current_sum)

        return max_sum
```

## Approach 2: Kadane’s Algorithm (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxSubArray(self, nums):
        max_sum = nums[0]
        current_sum = nums[0]

        # Traverse the array
        for i in range(1, len(nums)):
            # Update the current sum: either extend the subarray or start a new one
            current_sum = max(nums[i], current_sum + nums[i])

            # Update the maximum sum encountered so far
            max_sum = max(max_sum, current_sum)

        return max_sum
```

