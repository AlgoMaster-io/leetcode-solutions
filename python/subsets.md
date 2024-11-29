# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
```python
# Time Complexity: O(n * 2^n), where n is the number of elements
# Space Complexity: O(n * 2^n) for storing the subsets

class Solution:
    def subsets(self, nums):
        result = [[]]  # Start with the empty subset

        for num in nums:
            size = len(result)
            for i in range(size):
                subset = result[i] + [num]  # Add the current number to each existing subset
                result.append(subset)

        return result
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
```python
# Time Complexity: O(n * 2^n), where n is the number of elements
# Space Complexity: O(n) for the recursion stack

class Solution:
    def subsets(self, nums):
        result = []
        self.backtrack(nums, 0, [], result)
        return result

    def backtrack(self, nums, index, current, result):
        result.append(list(current))  # Add the current subset to the result

        for i in range(index, len(nums)):
            current.append(nums[i])  # Include nums[i] in the current subset
            self.backtrack(nums, i + 1, current, result)  # Recurse with the next index
            current.pop()  # Backtrack to explore other subsets
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
```python
# Time Complexity: O(n * 2^n), where n is the number of elements
# Space Complexity: O(n * 2^n) for storing the subsets

class Solution:
    def subsets(self, nums):
        result = []
        total_subsets = 1 << len(nums)  # Total subsets = 2^n

        for mask in range(total_subsets):
            subset = []
            for i in range(len(nums)):
                if (mask & (1 << i)) != 0:
                    subset.append(nums[i])  # Include nums[i] in the subset
            result.append(subset)

        return result
```

