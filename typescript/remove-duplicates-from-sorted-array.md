# [LeetCode Problem 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approaches
1. [Two Pointer Approach](#two-pointer-approach)

## Two Pointer Approach

### Intuition
In a sorted array, duplicates will always be adjacent. The idea is to use two pointers to help us identify unique elements and write them back into the array. We have a slow pointer `slow` that will track the position where we write the next unique element, and a fast pointer `fast` that will iterate through the array to find unique elements. This way, we overwrite duplicates in-place, and no extra space is required.

### Approach
1. Start by assigning the first element to the `slow` index since it is always unique.
2. Iterate over the array with `fast` starting from the second position.
3. If the current element pointed by `fast` is different from the element at `slow`, increment `slow` and assign the value at `fast` to `slow`.
4. Continue until `fast` traverses the entire array.
5. The value of `slow` after the loop points to the last unique element, so `slow + 1` will give the number of unique elements.

### Algorithm
```typescript
function removeDuplicates(nums: number[]): number {
    if (nums.length === 0) return 0;

    // Initialize the slow pointer
    let slow = 0;

    // Fast pointer iterates through the array
    for (let fast = 1; fast < nums.length; fast++) {
        // Compare the current fast element with the slow element
        if (nums[fast] !== nums[slow]) {
            // Increment slow and update its value to the current fast element
            slow++;
            nums[slow] = nums[fast];
        }
    }

    // The length of unique elements is slow index + 1
    return slow + 1;
}
```

### Important Steps
- **Initialization**: Start `slow` at the first index since the first element is always unique.
- **Checking uniqueness**: Compare current `fast` element with the `slow` element.
- **Updating indices**: Increment `slow` only if a new unique element is found at `fast`.

### Time Complexity
- **O(n)**: We traverse the array with the `fast` pointer, which takes linear time.

### Space Complexity
- **O(1)**: We are modifying the array in place without using extra space for another array.

By using the two-pointer method, we achieve an efficient in-place solution with minimal space usage and linear time complexity, making it optimal for this problem.

