# [15. 3Sum](https://leetcode.com/problems/3sum/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^3)
# Space Complexity: O(1) (excluding output list)
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = set()
        n = len(nums)

        # Iterate through all possible triplets
        for i in range(n - 2):
            for j in range(i + 1, n - 1):
                for k in range(j + 1, n):
                    if nums[i] + nums[j] + nums[k] == 0:
                        triplet = sorted([nums[i], nums[j], nums[k]])
                        result.add(tuple(triplet)) # Add triplet to the set to avoid duplicates

        return [list(triplet) for triplet in result] # Convert set to list for the output
```

## Approach 2: Two Pointers (Optimal)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n) (for sorting or output list)
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort() # Sort the array to use two-pointer technique

        for i in range(len(nums) - 2):
            # Skip duplicates for the first element
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, len(nums) - 1

            # Two-pointer approach
            while left < right:
                total = nums[i] + nums[left] + nums[right]
                if total == 0:
                    result.append([nums[i], nums[left], nums[right]])

                    # Skip duplicates for the second and third elements
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    left += 1
                    right -= 1
                elif total < 0:
                    left += 1 # Increase the sum
                else:
                    right -= 1 # Decrease the sum

        return result
```

