# [LeetCode Problem 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

## Solutions:

1. [Linear Search](#approach-1-linear-search)
2. [Binary Search](#approach-2-binary-search)

---

### Approach 1: Linear Search

#### Intuition:
The simplest approach is to iterate over the entire list and check if an element is greater than or equal to the target. As soon as we find such an element, we can return its index. If no such element is found, the target should be inserted at the end.

#### Algorithm:
1. Iterate through the list `nums`.
2. For each element, check if it is greater than or equal to `target`.
3. Return the current index if the condition is satisfied.
4. If the loop ends without returning, return the length of `nums`.

#### Code:

```python
def searchInsert(nums, target):
    for i in range(len(nums)):
        # If the current number is greater than or equal to target,
        # that means target should be inserted at index i.
        if nums[i] >= target:
            return i
    # If target is greater than all elements, it should be inserted at the end.
    return len(nums)
```

#### Time Complexity:
- **O(n)**: We may need to traverse the entire list in the worst case.
  
#### Space Complexity:
- **O(1)**: No additional space is used.

---

### Approach 2: Binary Search

#### Intuition:
As the list is sorted, we can use a binary search algorithm to find the appropriate insert position in logarithmic time. This involves dividing the problem into smaller sub-problems by halving the list each time.

#### Algorithm:
1. Initialize two pointers, `left` and `right`, starting at the beginning and end of the list, respectively.
2. While `left` is less than or equal to `right`:
   - Calculate `mid` as the average of `left` and `right`.
   - If `nums[mid]` matches `target`, return `mid`.
   - If `nums[mid]` is less than `target`, shift the `left` pointer to `mid + 1`.
   - Otherwise, shift the `right` pointer to `mid - 1`.
3. When `left` exceeds `right`, `left` will be the correct insert position for `target`.

#### Code:

```python
def searchInsert(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            # Target found, return its index
            return mid
        elif nums[mid] < target:
            # If mid value is less than target, move left pointer
            left = mid + 1
        else:
            # If mid value is greater than target, move right pointer
            right = mid - 1
    
    # If no exact match is found, left gives the insert position
    return left
```

#### Time Complexity:
- **O(log n)**: Binary search reduces the search space by half each step.
  
#### Space Complexity:
- **O(1)**: Only constant extra space is used. 

---

These solutions present a trade-off between simplicity and efficiency, with the binary search being the optimal one for this scenario due to its logarithmic time complexity in finding the insert position in a sorted list.

