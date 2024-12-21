# [Leetcode 4: Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approaches:
- [Approach 1: Merge and Sort](#approach-1-merge-and-sort)
- [Approach 2: Binary Search](#approach-2-binary-search)

### Approach 1: Merge and Sort

#### Intuition:
The simplest way to find the median of two sorted arrays is to merge them into one single sorted array and then find the median. This would be analogous to directly using the merge step of the merge sort algorithm.

#### Steps:
1. Merge the two sorted arrays.
2. Sort the merged array.
3. Calculate the median:
   - If the length of the merged array is odd, the median is the middle element.
   - If the length is even, the median is the average of the two middle elements.

```typescript
function findMedianSortedArrays(nums1: number[], nums2: number[]): number {
    // Merge both arrays
    const mergedArray = nums1.concat(nums2);

    // Sort the merged array
    mergedArray.sort((a, b) => a - b);

    const n = mergedArray.length;

    // Find the median
    if (n % 2 === 1) {
        // If odd, return the middle element
        return mergedArray[Math.floor(n / 2)];
    } else {
        // If even, return the average of the two middle elements
        return (mergedArray[(n / 2) - 1] + mergedArray[n / 2]) / 2;
    }
}
```

- **Time Complexity:** O((m+n) log(m+n)), where m and n are the lengths of the two arrays.
- **Space Complexity:** O(m+n) since we create a new combined array.

### Approach 2: Binary Search

#### Intuition:
This optimized approach uses binary search to find the partition point directly. The goal is to partition both arrays so that all elements in the left half are less than all elements in the right half.

#### Steps:
1. Ensure `nums1` is the smaller array to optimize the binary search range.
2. Utilize binary search on the smaller array:
   - Calculate the partition points for both arrays based on the middle index of `nums1`.
   - Check if these partition points make the conditions valid (i.e., left half's maximum is less than right half's minimum).
   - Adjust partitions based on the comparison outcomes.
3. Calculate the median using the median formula based on partition validity.

```typescript
function findMedianSortedArrays(nums1: number[], nums2: number[]): number {
    if (nums1.length > nums2.length) {
        // Swap the arrays so that nums1 is always smaller
        [nums1, nums2] = [nums2, nums1];
    }

    const m = nums1.length;
    const n = nums2.length;
    let low = 0, high = m;

    while (low <= high) {
        const partitionX = Math.floor((low + high) / 2);
        const partitionY = Math.floor((m + n + 1) / 2) - partitionX;

        const maxX = (partitionX === 0) ? Number.NEGATIVE_INFINITY : nums1[partitionX - 1];
        const maxY = (partitionY === 0) ? Number.NEGATIVE_INFINITY : nums2[partitionY - 1];

        const minX = (partitionX === m) ? Number.POSITIVE_INFINITY : nums1[partitionX];
        const minY = (partitionY === n) ? Number.POSITIVE_INFINITY : nums2[partitionY];

        if (maxX <= minY && maxY <= minX) {
            // Found the correct partition
            if ((m + n) % 2 === 0) {
                return (Math.max(maxX, maxY) + Math.min(minX, minY)) / 2;
            } else {
                return Math.max(maxX, maxY);
            }
        } else if (maxX > minY) {
            // Move towards the left in nums1
            high = partitionX - 1;
        } else {
            // Move towards the right in nums1
            low = partitionX + 1;
        }
    }

    throw new Error("Arrays are not sorted");
}
```

- **Time Complexity:** O(log(min(m, n))), where m and n are the lengths of the two arrays.
- **Space Complexity:** O(1), no additional space is used except for a few variables.

