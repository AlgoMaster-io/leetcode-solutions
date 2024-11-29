# 283. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def moveZeroes(self, nums):
        # Loop through the array
        for i in range(len(nums)):
            if nums[i] == 0:
                # Find the next non-zero element and swap
                for j in range(i + 1, len(nums)):
                    if nums[j] != 0:
                        # Swap the elements
                        nums[i], nums[j] = nums[j], nums[i]
                        break
```

## Approach 2: Two-Pointer Technique

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def moveZeroes(self, nums):
        lastNonZeroFoundAt = 0  # Index to place the next non-zero element

        # Move all non-zero elements to the front of the array
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[lastNonZeroFoundAt] = nums[i]
                lastNonZeroFoundAt += 1

        # Fill the remaining positions with zeros
        for i in range(lastNonZeroFoundAt, len(nums)):
            nums[i] = 0
```

## Approach 3: Optimal Two-Pointer with Swapping

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def moveZeroes(self, nums):
        left = 0  # Pointer to track the position of the next non-zero element

        # Iterate through the array
        for right in range(len(nums)):
            if nums[right] != 0:
                # Swap the non-zero element with the left pointer
                nums[left], nums[right] = nums[right], nums[left]
                left += 1  # Increment the left pointer
```

