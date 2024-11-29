# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def subarraySum(self, nums, k):
        count = 0

        # Check all subarrays
        for i in range(len(nums)):
            sum = 0

            for j in range(i, len(nums)):
                sum += nums[j]

                # If the sum equals k, increment the count
                if sum == k:
                    count += 1

        return count
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def subarraySum(self, nums, k):
        prefixSumMap = defaultdict(int)
        prefixSumMap[0] = 1  # Initialize with a prefix sum of 0

        count = 0
        sum = 0

        for num in nums:
            sum += num  # Update the prefix sum

            # Check if there's a prefix sum that makes the current sum equal to k
            if sum - k in prefixSumMap:
                count += prefixSumMap[sum - k]

            # Update the frequency of the current prefix sum
            prefixSumMap[sum] += 1

        return count
```

