# [Leetcode 128: Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

## Approaches

- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Using a Set and Linear Scan](#approach-2-using-a-set-and-linear-scan)

## Approach 1: Sorting

### Intuition
The simplest way to find the longest consecutive sequence in an unsorted array is to first sort the array. Once sorted, consecutive numbers will naturally appear next to each other. We can then iterate through the array, count the length of every consecutive sequence, and keep track of the longest one found.

### Algorithm
1. If the array is empty, return 0.
2. Sort the array.
3. Initialize variables `longest` to track the longest sequence and `current_streak` to 1 to track the current sequence length.
4. Iterate through the sorted array and for each number:
   - If it is consecutive to the previous one, increment the `current_streak`.
   - If it is not, update `longest` if `current_streak` is greater and reset `current_streak` to 1.
5. After the loop, ensure to update `longest` with the last streak.

### Code

```typescript
function longestConsecutive(nums: number[]): number {
    if (nums.length === 0) return 0;

    nums.sort((a, b) => a - b); // Sort the array
    let longest = 0;
    let current_streak = 1; // Start with a streak of 1

    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[i - 1]) { // Skip duplicates
            if (nums[i] === nums[i - 1] + 1) {
                // Consecutive number
                current_streak++;
            } else {
                // Sequence broke, update longest if needed
                longest = Math.max(longest, current_streak);
                current_streak = 1; // Reset streak
            }
        }
    }

    // At the end, compare the last streak
    return Math.max(longest, current_streak);
}
```

### Complexity
- Time Complexity: \(O(n \log n)\) due to sorting.
- Space Complexity: \(O(1)\) if not considering input storage, otherwise \(O(n)\) for sorting.

## Approach 2: Using a Set and Linear Scan

### Intuition
We can improve the efficiency by using a `Set`. The main idea is to achieve linear time complexity by avoiding sorting and instead, taking advantage of set membership tests. For each number, if it's the start of a sequence (i.e., `number - 1` is not in the array), then check the length of the sequence beginning with this number by incrementing and checking the presence of subsequent numbers in the set.

### Algorithm
1. Convert the array to a set, which allows \(O(1)\) average time complexity for membership checking.
2. Initialize `longest_streak` to zero.
3. Iterate over each number in the set:
   - If the current number can be the start of a sequence (i.e., `number - 1` is not in the set), start counting the sequence's length.
   - Incrementally check the presence of the next numbers and count the streak's length.
   - Update the `longest_streak` if the current streak is the longest encountered so far.
4. Return `longest_streak`.

### Code

```typescript
function longestConsecutive(nums: number[]): number {
    const numSet = new Set(nums); // Create a set to keep unique numbers
    let longest_streak = 0;

    for (let num of numSet) {
        // Only try to build sequences from the beginning of the sequence
        if (!numSet.has(num - 1)) {
            // If there's no smaller number in the set, this could be the start
            let current_num = num;
            let current_streak = 1;

            while (numSet.has(current_num + 1)) { // Check for next consecutive numbers
                current_num++;
                current_streak++;
            }

            // Update longest streak found so far
            longest_streak = Math.max(longest_streak, current_streak);
        }
    }
    
    return longest_streak;
}
```

### Complexity
- Time Complexity: \(O(n)\) because each number is processed at most twice.
- Space Complexity: \(O(n)\) to store the set of numbers.

