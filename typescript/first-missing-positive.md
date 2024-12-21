# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches
- [Brute Force with Sorting](#brute-force-with-sorting)
- [Using Hash Set](#using-hash-set)
- [Cyclic Sort Algorithm](#cyclic-sort-algorithm)


### Brute Force with Sorting

#### Intuition

The simplest approach involves sorting the array first and then scanning through it to find the first positive integer that is missing. Sorting makes it easier to detect sequences of consecutive numbers, but is not the most efficient way to find the solution.

#### Approach

1. **Sort the Array:** First, we sort the array. This brings all numbers in a particular order.
2. **Check the Positive Sequence:** After sorting, iterate through the array starting from 1, and check if each consecutive integer is present.
3. **Identify the Missing Integer:** The first integer that is missing in this sequence is our result.

```typescript
function firstMissingPositive(nums: number[]): number {
    // Step 1: Sort the array in ascending order
    nums.sort((a, b) => a - b);

    // Step 2: Start checking from 1 for the missing positive integer
    let missingPositive = 1;
    for (const num of nums) {
        // If current number is the missingPositive, increment missingPositive
        if (num === missingPositive) {
            missingPositive += 1;
        }
    }
    
    // The missingPositive is the first missing positive integer
    return missingPositive;
}
```

- **Time Complexity:** O(n log n) because of the sorting step.
- **Space Complexity:** O(1) for in-place sorting, ignoring the stack space for recursion or other function call overheads.


### Using Hash Set

#### Intuition

By using a hash set, we can efficiently check for the existence of integers. This approach eliminates the need for sorting by taking advantage of the average O(1) lookup time for set members.

#### Approach

1. **Populate a Set:** Add all positive integers from the array into a hash set.
2. **Iterate and Check:** Start from 1 and keep checking if the number exists in the set. The first missing number that is not found in the set is our answer.

```typescript
function firstMissingPositive(nums: number[]): number {
    // Step 1: Create a hash set containing only positive numbers
    const numSet = new Set<number>();

    // Add all positive numbers from the array into the set
    for (const num of nums) {
        if (num > 0) {
            numSet.add(num);
        }
    }

    // Step 2: Start checking from 1 for the first missing positive integer
    let missingPositive = 1;
    while (numSet.has(missingPositive)) {
        missingPositive += 1;
    }

    return missingPositive;
}
```

- **Time Complexity:** O(n) for creating the set and searching.
- **Space Complexity:** O(n) for storing numbers in the set.


### Cyclic Sort Algorithm

#### Intuition

The goal is to rearrange numbers in place within the array such that each number `num[i]` is in index `num[i] - 1`, if it's within the range `[1, n]`. During this process, misplaced numbers will lead us to identify the missing one.

#### Approach

1. **Cyclic Positioning:** Iterate over the array and attempt to place each number at its correct index position `num[i] - 1`.
2. **Final Inspection:** Scan all positions and find the first position where `num[i]` is not equal to `i + 1` to determine the first missing positive.

```typescript
function firstMissingPositive(nums: number[]): number {
    const n = nums.length;
    
    // Place each number in its rightful position, if it's within the range [1, n]
    for (let i = 0; i < n; i++) {
        while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] !== nums[i]) {
            // Swap numbers to put them at their correct positions
            [nums[nums[i] - 1], nums[i]] = [nums[i], nums[nums[i] - 1]];
        }
    }
    
    // Step 2: Identify the first missing positive number
    for (let i = 0; i < n; i++) {
        if (nums[i] !== i + 1) {
            return i + 1;
        }
    }
    
    // If all positions are filled correctly, the missing number is `n + 1`
    return n + 1;
}
```

- **Time Complexity:** O(n), as each number is placed exactly once.
- **Space Complexity:** O(1), since we modify the array in place without extra data structures.

