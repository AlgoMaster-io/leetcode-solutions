# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1) (ignoring the space required for sorting)
class Solution:
    def maximumGap(self, nums):
        if not nums or len(nums) < 2:
            return 0

        # Sort the array
        nums.sort()

        max_gap = 0

        # Calculate the maximum gap between consecutive elements
        for i in range(1, len(nums)):
            max_gap = max(max_gap, nums[i] - nums[i - 1])

        return max_gap
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def maximumGap(self, nums):
        if not nums or len(nums) < 2:
            return 0

        min_num, max_num = min(nums), max(nums)
        n = len(nums)

        if min_num == max_num:
            return 0

        # Calculate the gap size and number of buckets
        gap = (max_num - min_num) // (n - 1) + 1
        bucket_min = [float('inf')] * (n - 1)
        bucket_max = [float('-inf')] * (n - 1)

        # Place each number into a bucket
        for num in nums:
            if num == min_num or num == max_num:
                continue
            index = (num - min_num) // gap
            bucket_min[index] = min(bucket_min[index], num)
            bucket_max[index] = max(bucket_max[index], num)

        # Calculate the maximum gap
        max_gap, previous = 0, min_num
        for i in range(n - 1):
            if bucket_min[i] == float('inf'):
                continue  # Skip empty buckets
            max_gap = max(max_gap, bucket_min[i] - previous)
            previous = bucket_max[i]

        max_gap = max(max_gap, max_num - previous)  # Final gap with the max element

        return max_gap
```

