# [LeetCode 283: Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Two-Pointer Approach](#two-pointer-approach)
3. [Optimized Two-Pointer Approach](#optimized-two-pointer-approach)

### Brute Force Approach

**Intuition:**
In this approach, we create a new array for storing the non-zero elements and then add zeros to fill up the remaining places.

```typescript
function moveZeroes(nums: number[]): void {
    // Step 1: Store the non-zero elements in a new array
    const nonZeroes: number[] = [];
    
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nonZeroes.push(nums[i]);
        }
    }
    
    // Step 2: Update the original array with non-zero elements
    for (let i = 0; i < nonZeroes.length; i++) {
        nums[i] = nonZeroes[i];
    }
    
    // Step 3: Fill in the remaining part of the array with zeroes
    for (let i = nonZeroes.length; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

**Time Complexity:** O(n), where n is the number of elements in the array.  
**Space Complexity:** O(n), as we are using additional space to store the non-zero elements.

### Two-Pointer Approach

**Intuition:**
In this approach, we make use of two pointers: one to iterate over the array and another to track the position where the next non-zero element should be placed.

```typescript
function moveZeroes(nums: number[]): void {
    let position = 0; // This variable keeps track of the place to put the non-zero element
    
    // Step 1: Move all non-zero elements to the beginning of the array
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nums[position] = nums[i];
            position++;
        }
    }
    
    // Step 2: Fill the remaining part of the array with zeros
    for (let i = position; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

**Time Complexity:** O(n), as we are iterating the array twice in the worst-case scenario.  
**Space Complexity:** O(1), since we are modifying the array in-place without using extra space.

### Optimized Two-Pointer Approach

**Intuition:**
The previous two-pointer method can be further optimized by swapping elements in the array so that we can reduce the overall iterations.

```typescript
function moveZeroes(nums: number[]): void {
    let lastNonZeroFoundAt = 0; // Index of last found non-zero element

    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            // Swap the elements at indices i and lastNonZeroFoundAt
            [nums[lastNonZeroFoundAt], nums[i]] = [nums[i], nums[lastNonZeroFoundAt]];
            lastNonZeroFoundAt++; // Increment lastNonZeroFoundAt index
        }
    }
}
```

**Time Complexity:** O(n), since we are making one pass through the array.  
**Space Complexity:** O(1), as we are not using any extra space and modifying the array in-place.

This solution is optimal in terms of both time and space as it's straightforward, efficient, and handles all edge cases naturally.

