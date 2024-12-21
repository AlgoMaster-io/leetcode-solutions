# [LeetCode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches:
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: HashSet with Linear Time Complexity](#approach-2-hashset-with-linear-time-complexity)

---

### Approach 1: Sorting

**Intuition:**

The first straightforward approach to find the longest consecutive sequence is to sort the array. Once it is sorted, any consecutive sequence will appear in order. We can then iterate over the sorted array and count the length of consecutive numbers.

**Steps:**

1. Edge case: If the array is empty, return 0.
2. Sort the array.
3. Initialize two variables, `longestStreak` and `currentStreak`, to keep track of the maximum length of consecutive numbers and the current length of consecutive numbers, respectively.
4. Traverse the array from the second element to the end.
   - If the current number is different from the previous one and consecutive (i.e., `nums[i] == nums[i-1] + 1`), increment the `currentStreak`.
   - If it is not consecutive, reset the `currentStreak` to 1.
   - Update `longestStreak` if `currentStreak` is greater.
5. Return `longestStreak`.

```csharp
public int LongestConsecutive(int[] nums) {
    if (nums.Length == 0) return 0;

    Array.Sort(nums); // Step 2: Sort the array
    int longestStreak = 1;
    int currentStreak = 1;

    for (int i = 1; i < nums.Length; i++) { // Step 4: Traverse sorted array
        // If the current number is the same as the previous one, skip
        if (nums[i] == nums[i - 1]) continue;

        // Check if numbers are consecutive
        if (nums[i] == nums[i - 1] + 1) {
            currentStreak++; // Increment current streak
        } else {
            longestStreak = Math.Max(longestStreak, currentStreak); // Update longest streak
            currentStreak = 1; // Reset current streak
        }
    }

    return Math.Max(longestStreak, currentStreak); // Return the longest streak found
}
```

**Time Complexity:** O(n log n) - The sorting operation dominates the time complexity.  
**Space Complexity:** O(1) or O(n) depending on the sorting algorithm's space requirements.

---

### Approach 2: HashSet with Linear Time Complexity

**Intuition:**

To improve on the time complexity, we can leverage a HashSet to allow for fast look-ups. The idea is to hash all numbers into a set and then look for the start of a consecutive sequence. If a number x is the start of a sequence, `x-1` will not be in the set. We then expand the sequence by checking `x+1, x+2, ...` until the sequence ends.

**Steps:**

1. Edge case: If the array is empty, return 0.
2. Insert all elements into a `HashSet` to allow O(1) access for checking existence of elements.
3. Initialize `longestStreak` to track the maximum length of consecutive numbers.
4. Iterate over the set:
   - For each number, check if it is the start of a sequence by ensuring `num-1` is not in the set.
   - If it is, increment a counter for every consecutive number (`currentNum`), starting from this number until the sequence ends (i.e., `currentNum` no longer exists in the set).
   - Update `longestStreak` if the found sequence length is greater.
5. Return `longestStreak`.

```csharp
public int LongestConsecutive(int[] nums) {
    if (nums.Length == 0) return 0;

    HashSet<int> numSet = new HashSet<int>(nums); // Step 2: Insert all numbers into a set
    int longestStreak = 0;

    foreach (int num in numSet) { // Step 4: Iterate over the set
        // Check if it's the start of a sequence
        if (!numSet.Contains(num - 1)) {
            int currentNum = num;
            int currentStreak = 1;

            // Expand the sequence
            while (numSet.Contains(currentNum + 1)) {
                currentNum++;
                currentStreak++;
            }

            longestStreak = Math.Max(longestStreak, currentStreak); // Update longest streak if necessary
        }
    }

    return longestStreak; // Return the longest streak found
}
```

**Time Complexity:** O(n) - Each number is processed only once.  
**Space Complexity:** O(n) - For storing elements in the HashSet.

By using a HashSet and leveraging this clever property of sequences, we effectively reduce the time complexity to linear time, making this solution optimal for large datasets.

