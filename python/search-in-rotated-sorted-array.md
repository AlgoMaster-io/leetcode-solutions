[Leetcode Problem 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches:
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

### 1. Linear Search

Linear search is the most basic approach. We iterate through each element in the array and check if it matches the target.

#### Intuition:
The array is essentially a rotated version of a sorted array. A linear search will go through each element one by one until we find the target or exhaust the array without finding the target.

#### Implementation:

```python
def linear_search(nums, target):
    # Iterate through all elements in the array
    for i in range(len(nums)):
        # Check if the current element is the target
        if nums[i] == target:
            return i
    # If target is not found, return -1
    return -1

# Example usage:
# nums = [4,5,6,7,0,1,2], target = 0
# linear_search(nums, target) ➞ 4
```

#### Time Complexity:
- **O(n)**, where n is the number of elements in the array.

#### Space Complexity:
- **O(1)**, as no extra space is used.

### 2. Binary Search

The binary search method leverages the sorted property of the rotated array to find the target in logarithmic time.

#### Intuition:
A rotated sorted array divides into two sorted halves. By using the mid-point, we can determine which side is properly sorted and decide which side to discard in each iteration.

1. Compute `mid` index.
2. Check if the `mid` element is the target.
3. Determine if the left side is sorted (if `nums[left] <= nums[mid]`).
   - If the target is in the left sorted side, narrow the search to it.
   - Else, focus on the right unsorted side.
4. If the right side is sorted (`nums[mid] <= nums[right]`),
   - Do the converse of step 3.
5. Repeat until the target is found or the search space is exhausted.

#### Implementation:

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        # Check if we found the target
        if nums[mid] == target:
            return mid
        
        # Determine which side is sorted
        # If left side is sorted
        if nums[left] <= nums[mid]:
            # Check if the target is within the left sorted side
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # If right side is sorted
        else:
            # Check if the target is within the right sorted side
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
                
    return -1

# Example usage:
# nums = [4,5,6,7,0,1,2], target = 0
# binary_search(nums, target) ➞ 4
```

#### Time Complexity:
- **O(log n)**, where n is the number of elements in the array.

#### Space Complexity:
- **O(1)**, as only a few extra variables are used.

