# [Leetcode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Table of Contents
- [Approach 1: Linear Scan](#approach-1-linear-scan)
- [Approach 2: Binary Search](#approach-2-binary-search)

---

## Approach 1: Linear Scan

### Intuition
The simplest approach to solve this problem is to perform a linear scan across the array to determine the first and last positions of the target element. As we traverse the array, we update markers when we encounter the target element.

### Solution

```python
def searchRange(nums, target):
    # Initialize the variables to store the first and last positions
    first_position = -1
    last_position = -1

    # Iterate over the array
    for i in range(len(nums)):
        # Check if the current element is equal to the target
        if nums[i] == target:
            # If first_position is not updated (still -1), update it with current index
            if first_position == -1:
                first_position = i
            # Always update last_position with current index as it is the last occurrence found so far
            last_position = i

    return [first_position, last_position]

# Time Complexity: O(n), where n is the number of elements in the array. Each element is inspected once.
# Space Complexity: O(1), as no additional storage beyond a few variables is required.
```

---

## Approach 2: Binary Search

### Intuition
Given that the array is sorted, binary search is an efficient way to find the boundaries of the target value. We can perform two separate binary searches: one for finding the first occurrence and another for finding the last occurrence.

### Solution

```python
def searchRange(nums, target):
    def findFirst(nums, target):
        start, end = 0, len(nums) - 1
        while start <= end:
            mid = start + (end - start) // 2
            # Move the end pointer to mid - 1 when mid is greater or equal to target
            if nums[mid] >= target:
                end = mid - 1
            else:
                start = mid + 1
        # After the loop, start is at the first position or out of bounds
        # Verify if start is a valid position and matches the target
        if start < len(nums) and nums[start] == target:
            return start
        return -1

    def findLast(nums, target):
        start, end = 0, len(nums) - 1
        while start <= end:
            mid = start + (end - start) // 2
            # Move the start pointer to mid + 1 when mid is less or equal to target
            if nums[mid] <= target:
                start = mid + 1
            else:
                end = mid - 1
        # After the loop, end is at the last position or out of bounds
        # Verify if end is a valid position and matches the target
        if end >= 0 and nums[end] == target:
            return end
        return -1

    return [findFirst(nums, target), findLast(nums, target)]

# Time Complexity: O(log n) for each binary search, so O(log n) overall.
# Space Complexity: O(1), as no additional space is used beyond the indexes and loop variables.
```

In this approach, we take advantage of the sorted nature of the array to reduce the number of elements we examine. Each search operation either confirms or excludes portions of the array, quickly narrowing down to the desired range.

