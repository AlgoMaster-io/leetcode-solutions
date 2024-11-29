# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def maxProduct(self, nums):
        max_product = float('-inf')

        # Iterate through all possible subarrays
        for i in range(len(nums)):
            current_product = 1

            for j in range(i, len(nums)):
                current_product *= nums[j]
                max_product = max(max_product, current_product)

        return max_product
```

## Approach 2: Kadane's algorithm with Min/Max Tracking (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxProduct(self, nums):
        max_product = nums[0]
        current_max = nums[0]
        current_min = nums[0]

        # Traverse the array
        for num in nums[1:]:
            # Swap current_max and current_min if num is negative
            if num < 0:
                current_max, current_min = current_min, current_max

            # Update current_max and current_min
            current_max = max(num, current_max * num)
            current_min = min(num, current_min * num)

            # Update the global maximum product
            max_product = max(max_product, current_max)

        return max_product
```

