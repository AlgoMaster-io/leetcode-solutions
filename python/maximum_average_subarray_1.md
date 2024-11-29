# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n * k)
# Space Complexity: O(1)
class Solution:
    def findMaxAverage(self, nums, k):
        max_average = float('-inf')

        # Check all subarrays of size k
        for i in range(len(nums) - k + 1):
            current_sum = sum(nums[i:i + k])

            # Update the maximum average
            max_average = max(max_average, current_sum / k)

        return max_average
```

## Approach 2: Sliding Window (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def findMaxAverage(self, nums, k):
        current_sum = sum(nums[:k])

        max_average = current_sum / k

        # Slide the window over the array
        for i in range(k, len(nums)):
            current_sum += nums[i] - nums[i - k]  # Update the sum for the new window
            max_average = max(max_average, current_sum / k)  # Update the maximum average

        return max_average
```

