# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
```python
# Time Complexity: O(2^n)
# Space Complexity: O(n)
class Solution:
    def rob(self, nums):
        return self.robFrom(0, nums)

    def robFrom(self, index, nums):
        # Base case: If index is out of bounds, return 0
        if index >= len(nums):
            return 0

        # Either rob this house and skip one, or skip it
        robCurrent = nums[index] + self.robFrom(index + 2, nums)
        skipCurrent = self.robFrom(index + 1, nums)

        # Return the maximum of both choices
        return max(robCurrent, skipCurrent)
```

## Approach 2: Recursion with Memoization

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def rob(self, nums):
        return self.robFrom(0, nums, {})

    def robFrom(self, index, nums, memo):
        # Base case: If index is out of bounds, return 0
        if index >= len(nums):
            return 0

        # Check memoization table
        if index in memo:
            return memo[index]

        # Either rob this house and skip one, or skip it
        robCurrent = nums[index] + self.robFrom(index + 2, nums, memo)
        skipCurrent = self.robFrom(index + 1, nums, memo)

        # Memoize the result
        result = max(robCurrent, skipCurrent)
        memo[index] = result

        return result
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def rob(self, nums):
        if not nums:
            return 0

        dp = [0] * (len(nums) + 1)
        dp[0] = 0
        dp[1] = nums[0]

        # Build the dp array
        for i in range(2, len(nums) + 1):
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i - 1])

        return dp[len(nums)]
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def rob(self, nums):
        if not nums:
            return 0

        prev1 = 0  # max money can rob up to the previous house
        prev2 = 0  # max money can rob up to two houses before

        # Iterate through each house
        for num in nums:
            temp = prev1
            prev1 = max(prev2 + num, prev1)
            prev2 = temp

        return prev1
```

