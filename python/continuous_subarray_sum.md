# [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def checkSubarraySum(self, nums, k):
        n = len(nums)

        # Check every possible subarray
        for i in range(n - 1):
            total = nums[i]
            for j in range(i + 1, n):
                total += nums[j]
                if k == 0:
                    if total == 0:
                        return True
                elif total % k == 0:
                    return True

        return False  # No valid subarray found
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def checkSubarraySum(self, nums, k):
        remainder_map = {0: -1}  # Initialize with a remainder of 0 at index -1
        total = 0

        for i in range(len(nums)):
            total += nums[i]

            # Compute the remainder when divided by k
            remainder = total if k == 0 else total % k

            # If the remainder already exists
            if remainder in remainder_map:
                # Check if the subarray length is at least 2
                if i - remainder_map[remainder] > 1:
                    return True
            else:
                # Store the index of this remainder
                remainder_map[remainder] = i

        return False  # No valid subarray found
```

