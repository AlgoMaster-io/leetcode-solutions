## [Leetcode Problem 75: Sort Colors](https://leetcode.com/problems/sort-colors/)

### Approaches

1. [Counting Sort](#approach-1-counting-sort)
2. [One-Pass with Dutch National Flag Problem (Three Pointers)](#approach-2-one-pass-with-dutch-national-flag-problem-three-pointers)

---

### Approach 1: Counting Sort

Counting Sort is a simple, intuitive approach to handle the given problem because the problem guarantees only three different numbers (0, 1, 2). The idea is to count occurrences of each number and then overwrite the original array with the calculated counts.

#### Intuition

1. **Counting Occurrences**: Traverse through the array, maintaining counts of each color in a separate array, `count[]`.
   
2. **Overwrite Original Array**: Using the count of each color, overwrite the original array with the correct number of 0's, followed by 1's, and then 2's.

#### Code

```javascript
function sortColors(nums) {
    // Array to hold the count of each color
    let count = [0, 0, 0];

    // Count occurrences of each color
    for (let num of nums) {
        count[num]++;
    }

    // Overwrite nums array with sorted colors
    let index = 0;
    for (let i = 0; i < count[0]; i++) {
        nums[index++] = 0;
    }
    for (let i = 0; i < count[1]; i++) {
        nums[index++] = 1;
    }
    for (let i = 0; i < count[2]; i++) {
        nums[index++] = 2;
    }
}
```

#### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of elements in the array. We pass over the array twice (once to count, once to overwrite).
- **Space Complexity**: O(1), since we use a fixed-size count array.

---

### Approach 2: One-Pass with Dutch National Flag Problem (Three Pointers)

This approach is based on the Dutch National Flag Problem proposed by Edsger Dijkstra. It uses three pointers to sort the array in a single pass.

#### Intuition

1. **Three Pointers**: Use three pointers—`low`, `mid`, and `high`.
   - `low` tracks the boundary for 0's (elements less than 1).
   - `mid` traverses the array.
   - `high` tracks the boundary for 2's (elements greater than 1).

2. **Iterate and Swap**: Depending on the value of `nums[mid]`, perform swaps:
   - If `nums[mid]` is 0, swap `nums[low]` and `nums[mid]`, then increment both pointers.
   - If `nums[mid]` is 1, simply move the `mid` pointer.
   - If `nums[mid]` is 2, swap `nums[mid]` and `nums[high]`, then decrement `high`.
   - The `mid` is incremented only in the first two cases to ensure all 0s are at the start, and all 2s are at the end.

#### Code

```javascript
function sortColors(nums) {
    let low = 0, mid = 0, high = nums.length - 1;

    while (mid <= high) {
        if (nums[mid] === 0) {
            // Swap if current number is 0
            [nums[low], nums[mid]] = [nums[mid], nums[low]];
            low++;
            mid++;
        } else if (nums[mid] === 1) {
            // Move mid pointer if current number is 1
            mid++;
        } else { // nums[mid] === 2
            // Swap if current number is 2
            [nums[mid], nums[high]] = [nums[high], nums[mid]];
            high--;
        }
    }
}
```

#### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of elements in the array. Only a single pass is necessary.
- **Space Complexity**: O(1), since sorting is performed in-place without extra memory usage.

This approach efficiently sorts the colors in a single pass and minimal space, making it optimal for this problem.

