# 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach 1: Brute Force (Merge and Find Median)

### Solution
typescript
```typescript
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
class Solution {
    findMedianSortedArrays(nums1: number[], nums2: number[]): number {
        const m = nums1.length, n = nums2.length;
        const merged: number[] = new Array(m + n);

        let i = 0, j = 0, k = 0;
        while (i < m && j < n) {
            if (nums1[i] < nums2[j]) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }

        while (i < m) {
            merged[k++] = nums1[i++];
        }

        while (j < n) {
            merged[k++] = nums2[j++];
        }

        const totalLength = m + n;
        if (totalLength % 2 === 0) {
            return (merged[totalLength / 2 - 1] + merged[totalLength / 2]) / 2.0;
        } else {
            return merged[Math.floor(totalLength / 2)];
        }
    }
}
```

## Approach 2: Binary Search on Partition Index (Optimal Solution)

### Solution
typescript
```typescript
// Time Complexity: O(log(min(m, n)))
// Space Complexity: O(1)
class Solution {
    findMedianSortedArrays(nums1: number[], nums2: number[]): number {
        if (nums1.length > nums2.length) {
            return this.findMedianSortedArrays(nums2, nums1);
        }

        const m = nums1.length, n = nums2.length;
        let start = 0, end = m;

        while (start <= end) {
            const partitionX = Math.floor(start + (end - start) / 2);
            const partitionY = Math.floor((m + n + 1) / 2) - partitionX;

            const maxX = (partitionX === 0) ? Number.MIN_SAFE_INTEGER : nums1[partitionX - 1];
            const minX = (partitionX === m) ? Number.MAX_SAFE_INTEGER : nums1[partitionX];
            const maxY = (partitionY === 0) ? Number.MIN_SAFE_INTEGER : nums2[partitionY - 1];
            const minY = (partitionY === n) ? Number.MAX_SAFE_INTEGER : nums2[partitionY];

            if (maxX <= minY && maxY <= minX) {
                if ((m + n) % 2 === 0) {
                    return (Math.max(maxX, maxY) + Math.min(minX, minY)) / 2.0;
                } else {
                    return Math.max(maxX, maxY);
                }
            } else if (maxX > minY) {
                end = partitionX - 1;
            } else {
                start = partitionX + 1;
            }
        }

        throw new Error("Input arrays are not sorted or invalid.");
    }
}
```

