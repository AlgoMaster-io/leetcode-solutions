You can find the problem statement for Leetcode Problem 75: [Sort Colors](https://leetcode.com/problems/sort-colors/).

## Approaches

1. [Counting Sort Approach](#counting-sort-approach)
2. [One Pass (Dutch National Flag Algorithm)](#one-pass-dutch-national-flag-algorithm)

### Counting Sort Approach

The idea behind this approach is to count occurrences of each color and then overwrite the original array based on these counts. It's a simple and intuitive approach, often used when the input range is small and known, like in this problem where the colors are limited to 0, 1, and 2.

#### Intuition:
1. Count how many times each color appears.
2. Overwrite the input array with the correct number of 0s, 1s, and 2s based on these counts.

#### Steps:
1. Traverse the array and count the number of 0s, 1s, and 2s.
2. Overwrite the original array. First, write all 0s, then 1s, and finally 2s.

```typescript
function sortColors(nums: number[]): void {
    let count0 = 0, count1 = 0, count2 = 0;
    
    // Count the occurrences of 0s, 1s, and 2s
    nums.forEach(num => {
        if (num === 0) count0++;
        else if (num === 1) count1++;
        else if (num === 2) count2++;
    });

    // Overwrite the array based on counts
    for (let i = 0; i < nums.length; i++) {
        if (i < count0) nums[i] = 0;
        else if (i < count0 + count1) nums[i] = 1;
        else nums[i] = 2;
    }
}
```

- **Time Complexity**: O(n) - We iterate over the array twice: once for counting and once for overwriting.
- **Space Complexity**: O(1) - We use constant extra space for counting.

### One Pass (Dutch National Flag Algorithm)

The Dutch National Flag Algorithm developed by Edsger Dijkstra provides an optimal solution. It rearranges the array in a single pass with constant space.

#### Intuition:
1. Use three pointers: 
   - `low` to maintain the boundary of 0s.
   - `high` to maintain the boundary of 2s.
   - `i` to traverse through the array.
2. Swap elements to move 0s to the front and 2s to the back while 1s are left in the middle.

#### Steps:
1. Initialize `low` at start and `high` at end of the array.
2. Traverse the array with a pointer `i`:
   - If the element is 0, swap it with the element at `low` and increment both `low` and `i`.
   - If the element is 1, just move to the next element.
   - If the element is 2, swap it with the element at `high` and decrement `high` (do not increment `i`, as we need to check the swapped element).
   
```typescript
function sortColors(nums: number[]): void {
    let low = 0, high = nums.length - 1, i = 0;

    while (i <= high) {
        if (nums[i] === 0) {
            [nums[i], nums[low]] = [nums[low], nums[i]]; // Swap with low
            low++;
            i++;
        } else if (nums[i] === 1) {
            i++;
        } else {
            [nums[i], nums[high]] = [nums[high], nums[i]]; // Swap with high
            high--;
        }
    }
}
```

- **Time Complexity**: O(n) - Each element is processed at most once.
- **Space Complexity**: O(1) - Rearranges the array in place.

Both methods efficiently sort the array, but the second is more optimal in terms of operational passes and versatility for variety of problems.

