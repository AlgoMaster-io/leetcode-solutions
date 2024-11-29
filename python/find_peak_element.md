# 162. [Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approach 1: Linear Scan

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        # Traverse the array to find a peak
        for i in range(len(nums) - 1):
            # A peak is found if the current element is greater than the next
            if nums[i] > nums[i + 1]:
                return i  # Return the index of the peak
        return len(nums) - 1  # If no peak is found, the last element is a peak
```

## Approach 2: Binary Search

### Solution
```python
# Time Complexity: O(log n)
# Space Complexity: O(1)
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        start, end = 0, len(nums) - 1

        # Perform binary search to find the peak
        while start < end:
            mid = start + (end - start) // 2

            # If mid element is less than the next element, the peak lies on the right
            if nums[mid] < nums[mid + 1]:
                start = mid + 1  # Move to the right half
            else:
                # If mid element is greater than the next element, peak lies on the left
                end = mid  # Narrow search to the left half

        return start  # Start and end converge to the peak element
```

## Approach 3: Recursive Binary Search

### Solution
```python
# Time Complexity: O(log n)
# Space Complexity: O(log n) due to recursion stack
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        return self.search(nums, 0, len(nums) - 1)

    def search(self, nums: List[int], start: int, end: int) -> int:
        if start == end:
            return start  # Base case: single element is the peak

        mid = start + (end - start) // 2

        # Check if peak lies on the right half
        if nums[mid] < nums[mid + 1]:
            return self.search(nums, mid + 1, end)  # Recur for the right half
        else:
            return self.search(nums, start, mid)  # Recur for the left half
```

