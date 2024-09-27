# Search in Rotated Sorted Array
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

### Constraints:
- 1 <= nums.length <= 5000
- -10^4 <= nums[i] <= 10^4
- All values of nums are unique.
- nums is guaranteed to be rotated at some pivot.
- -10^4 <= target <= 10^4

### Examples
```javascript
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Input: nums = [1], target = 0
Output: -1
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
A brute-force approach would involve simply scanning through the entire array and returning the index of the target if it exists. This approach does not take advantage of the array being sorted (before rotation) or the O(log n) constraint.

Steps:
1. Iterate through the array nums.
2. If nums[i] == target, return the index i.
3. If the loop completes without finding the target, return -1.
##### Time Complexity:
O(n), where n is the length of the array.
##### Space Complexity:
O(1), as we only use a few variables.
##### Python Code:
```python
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```
### Approach 2: Binary Search with Pivot Handling (Optimal Solution)
##### Intuition: 
Since the array is rotated but still sorted, we can use a modified binary search to find the target. The key observation is that at least one half of the array (either the left half or the right half) is always sorted.

Steps:
1. Initialize two pointers: left = 0 and right = len(nums) - 1.
2. Perform a binary search:
   - Calculate the midpoint: mid = (left + right) // 2.
   - Check if nums[mid] == target. If yes, return mid.
   - If the left half is sorted (nums[left] <= nums[mid]:
     - If the target lies within the sorted left half (nums[left] <= target < nums[mid]), adjust right = mid - 1.
     - Otherwise, search in the right half (left = mid + 1).
   - If the right half is sorted (nums[mid] <= nums[right]):
     - If the target lies within the sorted right half (nums[mid] < target <= nums[right]), adjust left = mid + 1.
     - Otherwise, search in the left half (right = mid - 1).
3. If the target is not found, return -1.
##### Visualization:
For nums = [4, 5, 6, 7, 0, 1, 2] and target = 0:

```perl
Initial: left = 0, right = 6
Iteration 1: mid = 3 → nums[mid] = 7 → target is in the right half (search in [0, 1, 2])
Iteration 2: mid = 5 → nums[mid] = 1 → target is in the left half (search in [0, 0])
Iteration 3: mid = 4 → nums[mid] = 0 → found target at index 4
```
##### Time Complexity:
O(log n), where n is the length of the array. This is the same time complexity as binary search because we divide the search space in half at each step.
##### Space Complexity:
O(1), as we only use a few variables for the binary search process.
##### Python Code:
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        # Check if mid is the target
        if nums[mid] == target:
            return mid
        
        # Determine which half is sorted
        if nums[left] <= nums[mid]:  # Left half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1  # Target is in the left half
            else:
                left = mid + 1   # Target is in the right half
        else:  # Right half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1   # Target is in the right half
            else:
                right = mid - 1  # Target is in the left half
    
    return -1  # Target not found
```
### Edge Cases:
1. Single Element: If nums contains only one element, return 0 if the target equals the single element, otherwise return -1.
2. No Rotation: If the array is not rotated (i.e., already sorted), the solution still works because the binary search behaves normally.
3. Empty Array: If nums is empty, immediately return -1.
Target Not in Array: If the target is not present in the array, return -1 after the binary search completes.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n)      | O(1)             |
| Binary Search with Pivot Handling	                          | 	O(log x)            | O(1)             |

The Binary Search with Pivot Handling approach is the most efficient solution, achieving O(log n) time complexity while maintaining constant space. It efficiently uses binary search logic to handle the rotated nature of the array.