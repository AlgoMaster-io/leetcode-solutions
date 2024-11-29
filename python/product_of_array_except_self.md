# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approach 1: Left and Right Product Arrays

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def productExceptSelf(self, nums):
        n = len(nums)
        left = [0] * n
        right = [0] * n
        result = [0] * n

        # Compute left products
        left[0] = 1
        for i in range(1, n):
            left[i] = left[i - 1] * nums[i - 1]

        # Compute right products
        right[n - 1] = 1
        for i in range(n - 2, -1, -1):
            right[i] = right[i + 1] * nums[i + 1]

        # Compute result by multiplying left and right products
        for i in range(n):
            result[i] = left[i] * right[i]

        return result
```

## Approach 2: Optimized Space with Single Array

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1) (ignoring output array)
class Solution:
    def productExceptSelf(self, nums):
        n = len(nums)
        result = [0] * n

        # Compute left products directly in result
        result[0] = 1
        for i in range(1, n):
            result[i] = result[i - 1] * nums[i - 1]

        # Compute right products and multiply with left products
        right = 1
        for i in range(n - 1, -1, -1):
            result[i] *= right
            right *= nums[i]

        return result
```

