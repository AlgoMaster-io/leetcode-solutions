# [Leetcode 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approaches
- [Approach 1: Merge and Sort](#approach-1-merge-and-sort)
- [Approach 2: Two Pointers - Start from Left](#approach-2-two-pointers-start-from-left)
- [Approach 3: Two Pointers - Start from Right](#approach-3-two-pointers-start-from-right)

### Approach 1: Merge and Sort
This is the most straightforward approach. We simply copy elements from `nums2` to the end of `nums1`, then sort `nums1`. This approach takes advantage of the built-in sorting function but does not leverage the fact that both `nums1` and `nums2` are already sorted, leading to a less efficient solution.

#### Solution
```typescript
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    // Copy elements from nums2 into nums1 starting from index m
    for (let i = 0; i < n; i++) {
        nums1[m + i] = nums2[i];
    }
    // Sort nums1 in place
    nums1.sort((a, b) => a - b);
}
```

#### Complexity
- **Time Complexity**: O((m + n) log(m + n)) - The time complexity is dominated by the sort function.
- **Space Complexity**: O(1) - The sorting is done in-place without using additional space.

### Approach 2: Two Pointers - Start from Left
We use two pointers to iterate through `nums1` and `nums2` from the beginnings, and a third pointer to track the position of the merged results. This approach maintains the sorted order while using constant extra space.

#### Solution
```typescript
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    // Create an auxiliary array to store the merged result
    const merged = new Array(m + n);
    let i = 0, j = 0, k = 0;

    // Loop until we exhaust one of the arrays
    while (i < m && j < n) {
        if (nums1[i] < nums2[j]) {
            merged[k++] = nums1[i++];
        } else {
            merged[k++] = nums2[j++];
        }
    }

    // Copy any remaining elements from nums1
    while (i < m) {
        merged[k++] = nums1[i++];
    }

    // Copy any remaining elements from nums2
    while (j < n) {
        merged[k++] = nums2[j++];
    }

    // Copy the merged array back to nums1
    for (let l = 0; l < m + n; l++) {
        nums1[l] = merged[l];
    }
}
```

#### Complexity
- **Time Complexity**: O(m + n) - We traverse each list once.
- **Space Complexity**: O(m + n) - We use an extra array to store the merged result.

### Approach 3: Two Pointers - Start from Right
By using two pointers starting from the end of `nums1` and `nums2`, and another pointer from the end of the total length (m + n), we can avoid additional space usage and achieve an in-place merge. This is the optimal solution.

#### Solution
```typescript
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    let i = m - 1; // Last index for initial elements of nums1
    let j = n - 1; // Last index for elements of nums2
    let k = m + n - 1; // Last index for resulting merge in nums1

    // Start merging from the end of both arrays
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[k--] = nums1[i--];
        } else {
            nums1[k--] = nums2[j--];
        }
    }

    // If there are any elements left in nums2, add them to nums1
    while (j >= 0) {
        nums1[k--] = nums2[j--];
    }

    // No need to add remaining elements of nums1 since they are already in place
}
```

#### Complexity
- **Time Complexity**: O(m + n) - Similar to previous solutions, we traverse each list once.
- **Space Complexity**: O(1) - We modify `nums1` in place without using extra space. This is the most space-efficient approach.

