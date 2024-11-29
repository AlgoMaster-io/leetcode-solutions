# 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approach 1: Dynamic Programming

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
class Solution:
    def lengthOfLIS(self, nums):
        if not nums:
            return 0

        dp = [1] * len(nums)  # dp array to store the LIS ending at each index
        max_length = 1  # Minimum LIS length is 1 (a single element)

        # Build the dp array
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:  # Check if nums[i] can extend the subsequence at nums[j]
                    dp[i] = max(dp[i], dp[j] + 1)  # Update dp[i] with the maximum length found
            max_length = max(max_length, dp[i])  # Update the overall LIS length

        return max_length
```

## Approach 2: Binary Search with Patience Sorting

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from bisect import bisect_left

class Solution:
    def lengthOfLIS(self, nums):
        if not nums:
            return 0

        tails = []  # Array to store the smallest tail for all increasing subsequences

        for num in nums:
            index = bisect_left(tails, num)  # Binary search for the insertion point of num in tails

            # Replace or extend the increasing subsequence
            if index < len(tails):
                tails[index] = num
            else:
                tails.append(num)  # Append if it's a new tail

        return len(tails)
```

