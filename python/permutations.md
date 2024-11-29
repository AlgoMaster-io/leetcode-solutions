# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
python
```python
# Time Complexity: O(n * n!), where n is the length of the array
# Space Complexity: O(n * n!) for storing the permutations
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        visited = [False] * len(nums)
        self.generate_all(nums, [], result, visited)
        return result

    def generate_all(self, nums: List[int], current: List[int], result: List[List[int]], visited: List[bool]) -> None:
        if len(current) == len(nums):
            result.append(list(current))  # Add the current permutation
            return

        for i in range(len(nums)):
            if not visited[i]:
                visited[i] = True
                current.append(nums[i])  # Choose the current element
                self.generate_all(nums, current, result, visited)  # Recurse
                current.pop()  # Backtrack
                visited[i] = False  # Reset visited state
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(n * n!), where n is the length of the array
# Space Complexity: O(n * n!) for storing the permutations
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, [], result)
        return result

    def backtrack(self, nums: List[int], current: List[int], result: List[List[int]]) -> None:
        if len(current) == len(nums):
            result.append(list(current))  # Add the current permutation
            return

        for i in range(len(nums)):
            if nums[i] in current:
                continue  # Skip if the number is already in the current permutation
            current.append(nums[i])  # Choose the current element
            self.backtrack(nums, current, result)  # Recurse
            current.pop()  # Backtrack
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
python
```python
# Time Complexity: O(n * n!), where n is the length of the array
# Space Complexity: O(n) for the recursion stack
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, 0, result)
        return result

    def backtrack(self, nums: List[int], start: int, result: List[List[int]]) -> None:
        if start == len(nums):
            result.append(list(nums))  # Add the current permutation
            return

        for i in range(start, len(nums)):
            self.swap(nums, start, i)  # Swap to create a new permutation
            self.backtrack(nums, start + 1, result)  # Recurse
            self.swap(nums, start, i)  # Backtrack to the original state

    def swap(self, nums: List[int], i: int, j: int) -> None:
        nums[i], nums[j] = nums[j], nums[i]  # Swap two elements in the array
```

