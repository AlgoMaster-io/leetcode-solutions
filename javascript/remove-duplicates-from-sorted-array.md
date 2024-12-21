[Leetcode Problem 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Approaches:
1. [Two Pointer Approach](#two-pointer-approach)

### Two Pointer Approach

**Intuition:**
The problem asks us to remove duplicates from a sorted array such that each element appears only once and return the new length. Given that the array is sorted, duplicates will be adjacent. A common strategy in problems involving modifications within an array is to use the Two-Pointer technique.

In this approach:
- We will use two pointers: one, `i`, will track the current position of the non-duplicate element, and another, `j`, which will iterate over the array to find new unique elements.
- Initially, set `i` to 0, meaning the first element is always non-duplicate.
- Iterate over the array starting from the second element. If `nums[j]` is not equal to `nums[i]`, move `i` forward and set `nums[i]` to `nums[j]`. This updates the array in place with unique elements at the front.

This approach efficiently reduces duplicates in place with minimal space overhead, leveraging the fact that the input array is already sorted.

**Code:**

```javascript
function removeDuplicates(nums) {
    if (nums.length === 0) return 0; // Edge case: empty array
    let i = 0; // Initialize the first pointer

    for (let j = 1; j < nums.length; j++) {
        // If current element differs from the element at index i, it's unique
        if (nums[j] !== nums[i]) {
            i++; // Move the i pointer to the next position
            nums[i] = nums[j]; // Update the element at the new i position
        }
    }
    
    return i + 1; // Length is index + 1 since arrays are zero-indexed
}
```

**Time Complexity:** O(n)  
We only traverse through the list once with the `j` pointer, making it a linear complexity.

**Space Complexity:** O(1)  
The space used by the algorithm is constant because we're just using a few additional variables (`i` and `j`) and modifying the input array in place.

By applying the two-pointer technique, we efficiently remove duplicates and maintain unique values at the beginning of the array. This approach is optimal given the constraints and leverages the sorted nature of the array to work in place with linear time complexity.

