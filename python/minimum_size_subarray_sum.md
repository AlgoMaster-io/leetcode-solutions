# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        minLength = float('inf')

        # Iterate over all possible starting points
        for i in range(len(nums)):
            current_sum = 0

            # Iterate over all possible ending points
            for j in range(i, len(nums)):
                current_sum += nums[j]

                # Check if the sum is greater than or equal to the target
                if current_sum >= target:
                    minLength = min(minLength, j - i + 1)
                    break  # Break as the subarray length cannot decrease further

        return 0 if minLength == float('inf') else minLength
```

## Approach 2: Sliding Window (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left = 0
        current_sum = 0
        minLength = float('inf')

        # Sliding window
        for right in range(len(nums)):
            current_sum += nums[right]

            # Shrink the window until the sum is less than the target
            while current_sum >= target:
                minLength = min(minLength, right - left + 1)
                current_sum -= nums[left]
                left += 1

        return 0 if minLength == float('inf') else minLength
```

