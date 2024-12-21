# [Leetcode 283: Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal In-place Approach](#optimal-in-place-approach)

### Brute Force Approach

**Intuition**: 

The most straightforward way to solve the problem is to create a new array where we first append all non-zero elements from the original array, followed by appending zeroes to fill up the rest of the array to match the original array size. This ensures that all zeroes in the input are effectively "moved" to the end because we append them last.

**Steps**:
1. Create a new array `result` to store the non-zero elements.
2. Traverse the original array and add all non-zero elements to `result`.
3. Once we have added all the non-zero elements, calculate how many zeroes need to be added and fill up the remaining slots in `result`.
4. Copy the `result` array back into the original array to ensure the change is in-place.

**Code**:
```javascript
function moveZeroes(nums) {
    // create a new array to store non-zero elements
    let result = [];
    
    // first, add all non-zero elements to the new array
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            result.push(nums[i]);
        }
    }
    
    // calculate how many zeros are needed
    let zeroCount = nums.length - result.length;
    
    // append the zeros to the new array
    for (let i = 0; i < zeroCount; i++) {
        result.push(0);
    }
    
    // copy the result array back to the original array
    for (let i = 0; i < nums.length; i++) {
        nums[i] = result[i];
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n), where n is the length of the array, since we traverse the list three times.
- **Space Complexity**: O(n), because we utilize an extra array of the same length.

### Optimal In-place Approach

**Intuition**: 

To achieve an in-place solution, we can use a two-pointer technique. Start by keeping a pointer, `lastNonZeroFoundAt`, to track the position where the next non-zero element should be placed. By iterating through the array, whenever we encounter a non-zero, we swap it with the element at `lastNonZeroFoundAt` pointer and then increment this pointer. This ensures that all zeroes will end up at the rear of the array after a single pass through the list.

**Steps**:
1. Initialize `lastNonZeroFoundAt` to 0.
2. Traverse the list with a single iteration pointer `current`.
3. For every non-zero item encountered, swap it with the element at the `lastNonZeroFoundAt` index.
4. Increment `lastNonZeroFoundAt` for every swap ensuring notion of "advancing" to the next expected non-zero position. 

**Code**:
```javascript
function moveZeroes(nums) {
    // position to place the next non-zero element
    let lastNonZeroFoundAt = 0;
    
    // iterate through the array
    for (let current = 0; current < nums.length; current++) {
        // check if the current element is non-zero
        if (nums[current] !== 0) {
            // swap the elements at current position with the last non-zero position
            [nums[lastNonZeroFoundAt], nums[current]] = [nums[current], nums[lastNonZeroFoundAt]];
            // move to the next position for non-zero element
            lastNonZeroFoundAt++;
        }
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(n), with 'n' as the length of the array, as we iterate through the list once.
- **Space Complexity**: O(1), because we are performing operations in place without utilizing extra space.

