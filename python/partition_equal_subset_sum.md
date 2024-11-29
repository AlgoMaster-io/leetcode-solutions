# 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approach 1: Recursive Approach

### Solution
python
```python
# Time Complexity: O(2^n)
# Space Complexity: O(n)
class Solution:
    def canPartition(self, nums):
        total_sum = sum(nums)
        
        # If the sum is odd, partitioning into equal subsets is not possible
        if total_sum % 2 != 0:
            return False
        
        # Target is half of the total sum
        return self.can_partition(nums, total_sum // 2, 0)

    def can_partition(self, nums, target, index):
        # Base cases
        if target == 0:
            return True
        if target < 0 or index >= len(nums):
            return False
        
        # Recursive call to include or exclude the current element
        return self.can_partition(nums, target - nums[index], index + 1) or \
               self.can_partition(nums, target, index + 1)
```

## Approach 2: Dynamic Programming (DP) with 2D Array

### Solution
python
```python
# Time Complexity: O(n * target)
# Space Complexity: O(n * target)
class Solution:
    def canPartition(self, nums):
        total_sum = sum(nums)
        
        # Sum is odd so return false
        if total_sum % 2 != 0:
            return False
        
        target = total_sum // 2
        
        # Create a DP table to store previously computed results
        dp = [[False] * (target + 1) for _ in range(len(nums) + 1)]
        
        # Initialize first column, sum of 0 can be achieved without any elements
        for i in range(len(nums) + 1):
            dp[i][0] = True
        
        for i in range(1, len(nums) + 1):
            for j in range(1, target + 1):
                if j < nums[i - 1]:
                    dp[i][j] = dp[i - 1][j]
                else:
                    dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i - 1]]
        
        return dp[len(nums)][target]
```

## Approach 3: Dynamic Programming (DP) with 1D Array

### Solution
python
```python
# Time Complexity: O(n * target)
# Space Complexity: O(target)
class Solution:
    def canPartition(self, nums):
        total_sum = sum(nums)
        
        # If the sum is odd, partitioning into equal subsets is not possible
        if total_sum % 2 != 0:
            return False
        
        target = total_sum // 2
        
        # Create a DP array to hold boolean values
        dp = [False] * (target + 1)
        dp[0] = True  # Initial state; zero sum can always be achieved
        
        for num in nums:
            # Traverse from target backward to avoid overwriting
            for j in range(target, num - 1, -1):
                dp[j] = dp[j] or dp[j - num]
        
        return dp[target]
```

