# [Leetcode 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Hash Set](#approach-2-hash-set)
- [Approach 3: In-place Hashing (Cyclic Sort)](#approach-3-in-place-hashing-cyclic-sort)

### Approach 1: Sorting
The most straightforward way to solve this problem is by sorting the array first. Once sorted, we can iterate over the array and check for the first missing positive number.

#### Intuition:
1. Sort the array.
2. Traverse through the sorted array and find the first integer missing from `1` onwards.

```javascript
function firstMissingPositive(nums) {
    // Sort the array
    nums.sort((a, b) => a - b);

    // Initialize the first positive integer to check
    let missing = 1;

    for (let num of nums) {
        // If the number matches the current missing, increment
        if (num === missing) {
            missing++;
        }
    }

    return missing;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n log n) due to sorting.
- **Space Complexity:** O(1) if the sort is in-place, otherwise O(n).

### Approach 2: Hash Set
By using a hash set, we can store all positive numbers from the input and then iterate to find the first missing positive.

#### Intuition:
1. Store all positive numbers in a set.
2. Starting from `1`, check each number to see if it is in the set. The first number not found in the set is the answer.

```javascript
function firstMissingPositive(nums) {
    // Create a set from the numbers in the array
    const numSet = new Set(nums);

    // Start checking from 1 upwards
    let missing = 1;

    // Loop until we find a missing positive number
    while (numSet.has(missing)) {
        missing++;
    }

    return missing;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n) for inserting all elements into the set and checking for the missing number.
- **Space Complexity:** O(n) due to the storage of the numbers in the set.

### Approach 3: In-place Hashing (Cyclic Sort)
Instead of using extra space, place each number at its correct index (i.e., `number i` should be at index `i-1`). This method leverages the input array itself to track presence, making it optimal in terms of space complexity.

#### Intuition:
1. Swap numbers to their correct positions.
2. Iterate through the modified array to find the first position where the index `i` does not have the number `i+1`. That index gives the missing number `i+1`.

```javascript
function firstMissingPositive(nums) {
    const n = nums.length;

    // Place each number in its correct position
    for (let i = 0; i < n; i++) {
        while (
            nums[i] > 0 &&
            nums[i] <= n &&
            nums[nums[i] - 1] !== nums[i]
        ) {
            // Swap to place the number in the correct position
            let temp = nums[nums[i] - 1];
            nums[nums[i] - 1] = nums[i];
            nums[i] = temp;
        }
    }

    // After placing numbers correctly, find the first missing
    for (let i = 0; i < n; i++) {
        if (nums[i] !== i + 1) {
            return i + 1;
        }
    }

    // If all numbers from 1 to n are present, missing number is n+1
    return n + 1;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), as each number is placed at its correct position in constant time.
- **Space Complexity:** O(1), using the array itself for storage without extra space.

This problem showcases different algorithms from using additional space for simpler logic to optimizing space by rearranging the array cleverly with cycle sort. Each approach has its value depending on constraints like memory and performance requirements.

