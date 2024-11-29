# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
python
```python
# Time Complexity: O(2^n)
# Space Complexity: O(n)

class Solution:
    def findTargetSumWays(self, nums, target):
        return self.calculate(nums, 0, 0, target)
    
    def calculate(self, nums, i, cur_sum, target):
        # Base case: when we've considered all numbers
        if i == len(nums):
            # Return 1 if we've reached the target sum, 0 otherwise
            return 1 if cur_sum == target else 0
        
        # Explore both adding and subtracting the current number
        return self.calculate(nums, i + 1, cur_sum + nums[i], target) \
             + self.calculate(nums, i + 1, cur_sum - nums[i], target)
```

## Approach 2: Dynamic Programming with Memoization

### Solution
python
```python
# Time Complexity: O(n * target)
# Space Complexity: O(n * target)

class Solution:
    def findTargetSumWays(self, nums, target):
        # Use a dictionary to store calculated results for memoization
        return self.calculate(nums, 0, 0, target, {})
    
    def calculate(self, nums, i, cur_sum, target, memo):
        key = (i, cur_sum)
        
        # If result is already calculated for this state, return it
        if key in memo:
            return memo[key]
        
        # Base case: when we've considered all numbers
        if i == len(nums):
            return 1 if cur_sum == target else 0
        
        # Explore both adding and subtracting the current number
        add = self.calculate(nums, i + 1, cur_sum + nums[i], target, memo)
        subtract = self.calculate(nums, i + 1, cur_sum - nums[i], target, memo)
        
        # Save result in memo
        memo[key] = add + subtract
        return add + subtract
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
python
```python
# Time Complexity: O(n * target)
# Space Complexity: O(n * target)

class Solution:
    def findTargetSumWays(self, nums, target):
        sum_values = sum(nums)
        
        # If sum is smaller than target or (sum + target) is odd, return 0
        if abs(target) > sum_values or (sum_values + target) % 2 != 0:
            return 0
        
        s = (sum_values + target) // 2
        
        # dp[i] represents the number of ways to get the sum i
        dp = [0] * (s + 1)
        dp[0] = 1
        
        # Update dp array
        for num in nums:
            for j in range(s, num - 1, -1):
                dp[j] += dp[j - num]
        
        return dp[s]
```

