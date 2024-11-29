# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def sortColors(self, nums):
        count = [0] * 3
        
        # Count the occurrences of 0, 1, and 2
        for num in nums:
            count[num] += 1
        
        # Overwrite the array based on the counts
        index = 0
        for i in range(3):
            while count[i] > 0:
                nums[index] = i
                index += 1
                count[i] -= 1
```

## Approach 2: One-Pass (Dutch National Flag Algorithm)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def sortColors(self, nums):
        low, mid, high = 0, 0, len(nums) - 1
        
        while mid <= high:
            if nums[mid] == 0:
                # Swap nums[low] and nums[mid] and move pointers
                nums[low], nums[mid] = nums[mid], nums[low]
                low += 1
                mid += 1
            elif nums[mid] == 1:
                # Just move the mid pointer
                mid += 1
            else:
                # Swap nums[mid] and nums[high] and move high pointer
                nums[mid], nums[high] = nums[high], nums[mid]
                high -= 1
```

