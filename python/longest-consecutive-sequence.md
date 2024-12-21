# [Leetcode Problem - 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches

1. [Basic Approach - Sort and Check Consecutivity](#basic-approach---sort-and-check-consecutivity)
2. [Optimal Approach - Using HashSet](#optimal-approach---using-hashset)

---

## Basic Approach - Sort and Check Consecutivity

### Intuition

The simplest approach to solve the problem is to sort the array first, which allows us to identify consecutive elements by iterating through the sorted array. When the elements are sorted, the problem of finding the longest consecutive sequence becomes one of counting the longest sequence of `n, n+1, n+2, ...` etc, in the sorted array.

### Steps

1. **Sort the array**: Sorting allows us to bring potentially consecutive numbers together.
2. **Iterate through the sorted array**: Track the length of consecutive sequences and update the maximum length found.
3. **Edge cases**: Handle a case where the current number is equal to the previous. 

### Time and Space Complexity

- **Time Complexity**: O(n log n), because sorting the array takes O(n log n) and iterating through the array is O(n).
- **Space Complexity**: O(1) if we disregard the space used by the sort function (since it may require O(n) space).

### Solution Code

```python
def longestConsecutive(nums):
    if not nums:
        return 0

    nums.sort()
    longest_streak = 1
    current_streak = 1

    for i in range(1, len(nums)):
        # If current number is equal to the previous, skip it
        if nums[i] == nums[i - 1]:
            continue
        # If they are consecutive, increase current streak
        elif nums[i] == nums[i - 1] + 1:
            current_streak += 1
        else:
            # Reset the current streak and update the max streak
            longest_streak = max(longest_streak, current_streak)
            current_streak = 1
    
    # Update longest streak at the end of iteration
    return max(longest_streak, current_streak)
```

---

## Optimal Approach - Using HashSet

### Intuition

Sorting is not necessary to solve this problem. Instead, we can use a HashSet to quickly look up the existence of a number. The idea is to iterate through the array and for each number, try to build the longest consecutive sequence that starts with this number. 

To minimize redundant work, we only start building sequences for numbers that are the start of a sequence (i.e., `num - 1` is not in the set).

### Steps

1. **Add all numbers to a HashSet**: This provides O(1) average time complexity for lookups.
2. **Iterate through the numbers**: For each number, if `num - 1` is not in the set, this is the start of a sequence.
3. **Build the sequence**: Increment the streak while consecutive numbers are found in the set.
4. **Track longest streak**: Update the maximum length found.

### Time and Space Complexity

- **Time Complexity**: O(n), as each loop iteration that triggers a search costs at most the length of the sequence, and the whole process comprises O(n) numbers.
- **Space Complexity**: O(n), as we store all numbers in a set.

### Solution Code

```python
def longestConsecutive(nums):
    num_set = set(nums)
    longest_streak = 0

    for num in num_set:
        # Only start a new sequence if `num - 1` is not present
        if num - 1 not in num_set:
            current_num = num
            current_streak = 1

            # Keep increasing the streak while consecutive numbers are found
            while current_num + 1 in num_set:
                current_num += 1
                current_streak += 1

            # Update the maximum streak length if the current streak is longer
            longest_streak = max(longest_streak, current_streak)

    return longest_streak
```

By utilizing a HashSet and only starting sequences with the smallest elements, we efficiently solve the problem while maintaining a clear logic.

