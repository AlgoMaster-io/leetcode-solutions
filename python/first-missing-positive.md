# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches
1. [Basic Approach: Sorting and Scanning](#basic-approach-sorting-and-scanning)
2. [Optimal Approach: In-Place Hashing (Cyclic Sort)](#optimal-approach-in-place-hashing-cyclic-sort)

---

## Basic Approach: Sorting and Scanning

### Intuition
The straightforward approach is to sort the array and then look for the first missing positive. By sorting the array, all the numbers will be aligned in increasing order, allowing us to simply iterate over them to find the first missing positive number.

### Steps
1. Sort the array.
2. Initialize a variable `missing_pos` to 1.
3. Traverse through the sorted array:
   - If the current number equals `missing_pos`, increment `missing_pos`.
   - If the current number is greater than `missing_pos`, break the loop as we have found our answer.
4. The answer at the end of the loop is `missing_pos`.

### Code
```python
def firstMissingPositive(nums):
    nums.sort()  # Sort the numbers in-place.
    missing_pos = 1  # Initialize the first missing positive number to 1.
    for num in nums:
        if num == missing_pos:
            missing_pos += 1  # Found the current smallest missing positive, check for the next one.
        elif num > missing_pos:
            break  # No need to proceed further as we found the missing number.
    return missing_pos
```

### Complexity
- **Time Complexity**: O(n log n) due to the sorting operation.
- **Space Complexity**: O(1) if sorting is performed in-place, else O(n).

---

## Optimal Approach: In-Place Hashing (Cyclic Sort)

### Intuition
The optimal solution leverages the fact that the size of the input array determines the range of numbers we should check. A number `n` in an array of size `n` can only be from 1 to n for us to consider it as a potential minimum positive integer. Using this observation, we can rearrange the numbers in a way that each number is placed at an index corresponding to its value (i.e., 1 at index 0, 2 at index 1, etc.). This is similar to the cyclic sort pattern.

### Steps
1. Iterate over the array and place every number `num` at index `num - 1` if it is within the array bounds (i.e., 1 <= num <= n).
2. Once all numbers are placed, iterate over the array.
3. The first index where `nums[i]` is not `i + 1` gives the missing positive integer.
4. If all indices are correct, the missing integer is `n + 1`.

### Code
```python
def firstMissingPositive(nums):
    n = len(nums)
    for i in range(n):
        while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
            # Swap numbers to their correct position.
            nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
    
    # If after rearrangement, nums[i] != i + 1, i + 1 is the missing number.
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1

    # If all numbers are in their correct positions, the missing number is n+1
    return n + 1
```

### Complexity
- **Time Complexity**: O(n) because each number is placed at its correct index at most once.
- **Space Complexity**: O(1) since we do not use any additional data structures. All operations are performed in-place.

This optimal approach efficiently handles finding the smallest missing positive integer without the need to sort the array, making it highly suitable for large inputs.

