# 34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approach 1: Linear Search

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def searchRange(self, nums, target):
        first, last = -1, -1

        # Traverse the array to find the first and last positions
        for i in range(len(nums)):
            if nums[i] == target:
                if first == -1:
                    first = i  # Mark the first occurrence
                last = i  # Continuously update the last occurrence

        return [first, last]
```

## Approach 2: Binary Search (Two Separate Searches)

### Solution
python
```python
# Time Complexity: O(log n)
# Space Complexity: O(1)
class Solution:
    def searchRange(self, nums, target):
        first = self.findPosition(nums, target, True)   # Find first occurrence
        last = self.findPosition(nums, target, False)  # Find last occurrence
        return [first, last]

    def findPosition(self, nums, target, findFirst):
        start, end = 0, len(nums) - 1
        position = -1

        while start <= end:
            mid = start + (end - start) // 2

            if nums[mid] == target:
                position = mid  # Record the position
                if findFirst:
                    end = mid - 1  # Narrow search to the left
                else:
                    start = mid + 1  # Narrow search to the right
            elif nums[mid] < target:
                start = mid + 1  # Search in the right half
            else:
                end = mid - 1  # Search in the left half

        return position
```

## Approach 3: Optimized Binary Search (Combined Search for Both Positions)

### Solution
python
```python
# Time Complexity: O(log n)
# Space Complexity: O(1)
class Solution:
    def searchRange(self, nums, target):
        result = [-1, -1]
        if not nums:
            return result  # Return immediately if array is empty
        
        # Find the first occurrence
        start, end = 0, len(nums) - 1
        while start <= end:
            mid = start + (end - start) // 2
            if nums[mid] < target:
                start = mid + 1  # Search in the right half
            else:
                end = mid - 1  # Search in the left half
            if nums[mid] == target:
                result[0] = mid  # Update first position
        
        # Reset variables to find the last occurrence
        start, end = 0, len(nums) - 1
        while start <= end:
            mid = start + (end - start) // 2
            if nums[mid] <= target:
                start = mid + 1  # Search in the right half
            else:
                end = mid - 1  # Search in the left half
            if nums[mid] == target:
                result[1] = mid  # Update last position
        
        return result
```

