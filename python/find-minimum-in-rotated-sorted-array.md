# [Leetcode 153: Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Approaches:
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)


## Approach 1: Linear Search

### Intuition:
The simplest way to solve this problem is by traversing through each element in the array and identifying the minimum value. Since the array is rotated, the minimum value will be the point where the sequence order is disturbed (i.e., each element should be greater than or equal to the previous one except the rotated point).

### Code:
```python
def findMin(nums):
    # Initialize minimum value as the first element
    min_value = nums[0]
    
    # Loop through each number in the array to find the minimum
    for num in nums:
        if num < min_value:
            min_value = num  # Update minimum if a smaller number is found
            
    return min_value

# Example usage:
# nums = [3, 4, 5, 1, 2]
# The minimum element is 1
```

### Time Complexity:
- **O(n)**: We traverse through all elements in the array once.

### Space Complexity:
- **O(1)**: No additional space is used apart from a few variables.


## Approach 2: Modified Binary Search

### Intuition:
Given the nature of the problem where a sorted array is rotated, we can leverage binary search to find the minimal element in O(log n) time. The main idea here is to utilize the properties of a rotated sorted array, where one half will always be sorted while the other might contain the pivot to the smallest element. By comparing the middle element with the bounds, we can decide which half of the array to discard, thereby zeroing in on the minimum element efficiently.

### Code:
```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        # If mid element is greater than the rightmost element, 
        # then the smallest value must be in the right half.
        if nums[mid] > nums[right]:
            left = mid + 1
        else:
            # Otherwise, the smallest value is in the left half including mid
            right = mid
    
    # When left == right, we have found the smallest value
    return nums[left]

# Example usage:
# nums = [4, 5, 6, 7, 0, 1, 2]
# The minimum element is 0
```

### Time Complexity:
- **O(log n)**: We effectively halve the array at each step, thus achieving logarithmic complexity.

### Space Complexity:
- **O(1)**: The algorithm uses a constant amount of space regardless of input size.

Both approaches are correct, but the binary search method is much more efficient for larger arrays due to its logarithmic time complexity.

