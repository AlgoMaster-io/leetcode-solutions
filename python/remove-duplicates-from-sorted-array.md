# [Leetcode 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approaches
- [Approach 1: Using Additional Space](#approach-1-using-additional-space)
- [Approach 2: Two Pointers](#approach-2-two-pointers)

### Approach 1: Using Additional Space

**Intuition:**

The problem requires us to keep only unique elements in the sorted array. A naive approach is to utilize additional space where we store these unique elements.

1. Traverse through the array and keep track of the last seen unique element.
2. For each new element that is different from the last seen one, add it to a new list.
3. Once we finish, we copy back the unique elements to the original array.

This approach is straightforward but violates the problem's constraints regarding additional space.

**Solution:**

```python
def remove_duplicates(nums):
    if not nums:
        return 0
    
    unique_nums = [nums[0]]  # Start with the first element
    
    for num in nums[1:]:  # Start checking from the second element
        if num != unique_nums[-1]:  # Only add if current number is different
            unique_nums.append(num)
    
    # Copy back to nums the elements of unique_nums
    for i in range(len(unique_nums)):
        nums[i] = unique_nums[i]
    
    return len(unique_nums)
```

**Time Complexity:** O(n), where n is the length of the array, as we go through the array once and additional space is also filled in linear time.

**Space Complexity:** O(n) due to the additional list used for storing unique elements.

---

### Approach 2: Two Pointers

**Intuition:**

To solve the problem in-place, we can employ the two-pointer technique. 

1. **Pointer `i`:** traverses each element in the array.
2. **Pointer `j`:** keeps track of where the next unique element should be placed.

The idea is to iterate over the array with `i`, and whenever a new unique element is found (i.e., `nums[i] != nums[j]`), we increment `j` and update `nums[j]` with `nums[i]`. This effectively shifts unique elements towards the beginning of the array one by one.

**Solution:**

```python
def remove_duplicates(nums):
    if not nums:
        return 0
    
    j = 0  # Initialize the 'unique' end at the first index
    
    for i in range(1, len(nums)):  # Start checking from the second element
        if nums[i] != nums[j]:  # If a unique element is found
            j += 1  # Move the unique index up
            nums[j] = nums[i]  # Set the unique element at index j
            
    return j + 1  # Return the number of unique elements
```

**Time Complexity:** O(n), where n is the length of the array. We only traverse the array once.

**Space Complexity:** O(1) because we only use a constant amount of extra space (the variables `i` and `j`).

---

This set of solutions provides a clear, detailed approach from a naive implementation to an optimal one, following constraints and efficiency considerations.

