# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)

## Approach 1: Linear Search

### Intuition
The most straightforward approach is to iterate through the array and compare each element with the target. This is a simple solution but does not take advantage of the array's properties.

### Implementation

```typescript
function search(nums: number[], target: number): number {
    // Iterate through the array
    for (let i = 0; i < nums.length; i++) {
        // Check if the current element matches the target
        if (nums[i] === target) {
            return i; // Return the index if the target is found
        }
    }
    return -1; // Return -1 if the target is not found
}
```

### Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of elements in the array. We may need to check each element in the worst case.
- **Space Complexity**: O(1), as we are using a constant amount of extra space.

## Approach 2: Modified Binary Search

### Intuition
Given that the array is a rotated version of a sorted array, we can still use a modified binary search. The key is to identify and leverage the "sorted" nature of parts of the array:

1. Use binary search logic to split the array into two halves.
2. Determine which half is sorted.
3. Check if the target is within the sorted half. If it is, search within it, otherwise search in the other half.

### Implementation

```typescript
function search(nums: number[], target: number): number {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right) {
        const mid = Math.floor((left + right) / 2);

        // Check if the middle element is the target
        if (nums[mid] === target) {
            return mid;
        }

        // Determine if the left half is sorted
        if (nums[left] <= nums[mid]) {
            // Check if the target is in the range of the left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1; // Move the right boundary to mid - 1
            } else {
                left = mid + 1; // Move the left boundary to mid + 1
            }
        } else { // If the left half isn't sorted, the right half must be
            // Check if the target is in the range of the right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1; // Move the left boundary to mid + 1
            } else {
                right = mid - 1; // Move the right boundary to mid - 1
            }
        }
    }

    return -1; // Return -1 if the target is not found
}
```

### Time and Space Complexity

- **Time Complexity**: O(log n), where n is the number of elements in the array. The binary search reduces the search space by half each iteration.
- **Space Complexity**: O(1), as the algorithm uses a constant amount of extra space.

