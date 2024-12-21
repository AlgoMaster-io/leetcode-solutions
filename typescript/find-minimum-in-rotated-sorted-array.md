# [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search Approach](#binary-search-approach)

---

### Brute Force Approach

The simplest way to solve the problem involves iterating over the entire array and finding the minimum element. Although not the most efficient, it ensures a correct answer.

**Intuition**:
- Traverse each element in the array.
- Keep track of the minimum element encountered so far.
- Since arrays are rotated versions of a sorted array, the minimum element is the point at which the array starts.

**Algorithm**:
1. Initialize a variable, `minVal`, to a very large value (or to `nums[0]`).
2. Loop through each element in the array:
   - Update `minVal` if the current element is smaller than `minVal`.
3. Return `minVal` at the end of the loop.

**Time Complexity**: O(n) - We are iterating through the array once.

**Space Complexity**: O(1) - We are using a constant amount of extra space.

```typescript
function findMinBruteForce(nums: number[]): number {
    // Initialize the minimum value with the first element of the array
    let minVal = nums[0];
    
    // Traverse through the array to find the minimum element
    for (let i = 1; i < nums.length; i++) {
        // Update minVal if current element is smaller
        if (nums[i] < minVal) {
            minVal = nums[i];
        }
    }
    
    // Return the minimum value found
    return minVal;
}
```

---

### Binary Search Approach

A more efficient way utilizes binary search to find the minimum in a rotated sorted array. Since the array is sorted and then rotated, we can leverage this sorted property to reduce our search space.

**Intuition**:
- Utilize the properties of binary search but tailor it for a rotated sorted array.
- If the middle element is greater than the rightmost, the minimum is in the right half.
- If the middle element is less than the rightmost, the minimum is in the left half (or might be the middle itself).

**Algorithm**:
1. Initialize two pointers, `left` and `right`, to the beginning and end of the array, respectively.
2. While `left` pointer is less than the `right` pointer:
   - Calculate the middle index `mid`.
   - If `nums[mid]` is greater than `nums[right]`, the minimum value is in the right half (`left = mid + 1`).
   - Otherwise, it's in the left half, so include `mid` in the search (`right = mid`).
3. When `left` meets `right`, the smallest value is found.

**Time Complexity**: O(log n) - We halve the search space with each step.

**Space Complexity**: O(1) - No extra space is used beyond a few variables.

```typescript
function findMinBinarySearch(nums: number[]): number {
    // Initialize pointers to the start and end of the array
    let left = 0;
    let right = nums.length - 1;
    
    // Perform binary search
    while (left < right) {
        // Calculate the middle index
        let mid = Math.floor((left + right) / 2);
        
        // Compare middle element with the rightmost element
        if (nums[mid] > nums[right]) {
            // If nums[mid] is greater, the minimum is to the right
            left = mid + 1;
        } else {
            // Otherwise, the minimum is in the left side including mid
            right = mid;
        }
    }
    // When pointers converge, left will point to the minimum
    return nums[left];
}
```

Both approaches correctly solve the problem, but the binary search approach is highly efficient, taking advantage of the sorted property of the array.

