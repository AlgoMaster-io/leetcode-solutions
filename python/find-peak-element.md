# [Leetcode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

## Approaches:
1. [Linear Scan](#linear-scan)
2. [Recursive Binary Search](#recursive-binary-search)
3. [Iterative Binary Search](#iterative-binary-search)

### 1. Linear Scan

Intuition:
- The simplest way to solve this problem is to scan through the list from the beginning to the end.
- A peak element is defined as an element that is greater than its neighbors. With this definition, we can just compare each element with its immediate neighbors to determine if it is a peak.
- This approach will result in finding the first peak element we encounter.

```python
def findPeakElement(nums):
    # Check each element to see if it is greater than its neighbors
    for i in range(len(nums) - 1):
        # If the current element is greater than the next, it is a peak
        if nums[i] > nums[i + 1]:
            return i
    # If no peak is found, the last element is the peak by list constraints
    return len(nums) - 1

# Time Complexity: O(n), where n is the number of elements in the array.
# Space Complexity: O(1), only constant space is used.
```

### 2. Recursive Binary Search

Intuition:
- The idea of binary search allows us to find a solution faster by eliminating half the search space at every step.
- We identify a direction in which to move by comparing the middle element with its right neighbor. If `mid < mid + 1`, then there must be a peak on the right (including mid + 1).
- If `mid > mid + 1`, then there must be a peak on the left (including mid).

```python
def findPeakElement(nums):
    def search(left, right):
        # Base case: If the search space is reduced to one element
        if left == right:
            return left
        
        mid = (left + right) // 2
        
        # If the middle element is greater than its next element,
        # then a peak must be on the left side
        if nums[mid] > nums[mid + 1]:
            return search(left, mid)
        # Otherwise, peak must be on the right side
        else:
            return search(mid + 1, right)

    return search(0, len(nums) - 1)

# Time Complexity: O(log n), where n is the number of elements in the array.
# Space Complexity: O(log n), due to recursive function call stack.
```

### 3. Iterative Binary Search

Intuition:
- Similar to the recursive approach, but implemented iteratively to avoid recursion stack overhead.
- Continually update the search space by checking if the middle element forms a peak.
- Move towards the side where a potential peak could exist until the left and right pointers converge.

```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        
        # Compare mid and mid + 1 to determine the direction to move
        if nums[mid] > nums[mid + 1]:
            right = mid
        else:
            left = mid + 1
    
    # When left equals right, we have found a peak
    return left

# Time Complexity: O(log n), where n is the number of elements in the array.
# Space Complexity: O(1), no additional space is used.
```

In conclusion, all these methods are valid for finding a peak. The binary search solutions are much more optimal than a linear scan, providing logarithmic time complexity, which makes them suitable for larger inputs.

