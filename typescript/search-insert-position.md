# [Leetcode 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

In this problem, we are given a sorted array and a target value. We need to find the index at which the target value should be inserted to maintain the sorted order. If the target already exists in the array, return its index.

## Approaches
1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

### Linear Search

The simplest method is to linearly scan through the array and find the position for the target. This approach is straightforward but not optimal for larger arrays.

#### Intuition
We iterate through each element of the array and compare it with the target:
- If we find an element equal to the target, we've found our position.
- If we find an element greater than the target, the target should be inserted at or before this position.

#### Solution

```typescript
function searchInsertLinear(nums: number[], target: number): number {
    // Iterate over the array
    for (let i = 0; i < nums.length; i++) {
        // If the current element is greater than or equal to target, return its index
        if (nums[i] >= target) {
            return i;
        }
    }
    // If not found, the target should be inserted at the end of the array
    return nums.length;
}
```

#### Complexity
- **Time Complexity:** \(O(n)\) - We potentially look at each element once.
- **Space Complexity:** \(O(1)\) - No additional space is used, just iterating through the array.

### Binary Search

A more efficient method is to use binary search because the array is sorted.

#### Intuition
Since the array is sorted, we can utilize the binary search technique to efficiently find the position:
- Use two pointers, start and end, to define the range of the search.
- Calculate the middle index and compare the middle element with the target:
  - If the middle element is less than the target, move the start to `mid + 1`.
  - If the middle element is greater than the target, move the end to `mid - 1`.
- If the exact element is found, return `mid`. If not found, start will be at the insertion position due to the binary search logic.

#### Solution

```typescript
function searchInsertBinary(nums: number[], target: number): number {
    let start = 0;
    let end = nums.length - 1;

    while (start <= end) {
        let mid = Math.floor((start + end) / 2);

        // If target is found, return mid
        if (nums[mid] === target) {
            return mid;
        }
        // If target is greater than mid, ignore left half
        else if (nums[mid] < target) {
            start = mid + 1;
        }
        // If target is smaller than mid, ignore right half
        else {
            end = mid - 1;
        }
    }
    // If not found, start is the insertion point
    return start;
}
```

#### Complexity
- **Time Complexity:** \(O(\log n)\) - We divide the search interval in half every time, hence logarithmic.
- **Space Complexity:** \(O(1)\) - Additional space usage is minimum.

By implementing these approaches, you will have both a basic understanding and an optimized solution for finding the insertion position in a sorted array.

