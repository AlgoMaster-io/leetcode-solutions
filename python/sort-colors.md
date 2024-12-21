# [LeetCode 75: Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approaches
- [Approach 1: Counting Sort](#approach-1-counting-sort)
- [Approach 2: One-Pass with Three Pointers (Dutch National Flag Algorithm)](#approach-2-one-pass-with-three-pointers-dutch-national-flag-algorithm)

---

## Approach 1: Counting Sort

### Intuition
Since the problem deals with sorting a small finite set of numbers (0, 1, and 2), a counting sort is efficient. The idea is to count the number of occurrences of each number, and then rewrite the array based on the counts.

### Steps
1. Count the number of 0s, 1s, and 2s in the array.
2. Overwrite the original array based on these counts.

### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of elements in the array. We count each element and then rebuild the array.
- **Space Complexity:** O(1) since we only use a fixed amount of extra space for counters.

### Python Code
```python
def sortColors(nums):
    # Initialize counts for each color.
    count0, count1, count2 = 0, 0, 0
    
    # Count occurrence of each color.
    for num in nums:
        if num == 0:
            count0 += 1
        elif num == 1:
            count1 += 1
        else:
            count2 += 1
    
    # Rewrite the array based on counted frequencies.
    nums[:count0] = [0] * count0
    nums[count0:count0+count1] = [1] * count1
    nums[count0+count1:] = [2] * count2

    # Print the final sorted array for debugging purposes:
    # print(nums)
```

---

## Approach 2: One-Pass with Three Pointers (Dutch National Flag Algorithm)

### Intuition
In this approach, we use three pointers: `low`, `mid` and `high`. The idea is to partition the array into three sections: less than 1, equal to 1, and greater than 1 (which automatically means `high` to `end` is 2s since we're only dealing with 0, 1, and 2). By incrementing or decrementing the pointers based on the current element, we sort the array in one pass.

### Steps
1. Initialize three pointers: `low` at the start, `mid` at the start, and `high` at the end.
2. As we iterate with `mid` pointer:
   - If `nums[mid]` is 0, swap `nums[mid]` with `nums[low]`, increment both `low` and `mid`.
   - If `nums[mid]` is 1, increment `mid`.
   - If `nums[mid]` is 2, swap `nums[mid]` with `nums[high]`, decrement `high`.

### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of elements. Each element is processed at most once.
- **Space Complexity:** O(1) as we sort the array in-place using the three pointers.

### Python Code
```python
def sortColors(nums):
    # Initialize pointers
    low, mid = 0, 0
    high = len(nums) - 1
    
    # Perform swaps based on the current mid value
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]  # Swap 0-found to low position
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[mid], nums[high] = nums[high], nums[mid]  # Swap 2-found to high position
            high -= 1
    
    # Print the final sorted array for debugging purposes:
    # print(nums)
```

By using this two-pointer approach, the problem is efficiently solved in a single scan of the array, ensuring optimal performance both in terms of time and space.

