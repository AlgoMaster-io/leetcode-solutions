# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approach 1: Using Built-in Sorting (Simplest)

### Solution
typescript
```typescript
// Time Complexity: O((m + n) log(m + n))
// Space Complexity: O(1)
class Solution {
    merge(nums1: number[], m: number, nums2: number[], n: number): void {
        // Copy nums2 into nums1
        nums1.splice(m, n, ...nums2);

        // Sort the combined array
        nums1.sort((a, b) => a - b);
    }
}
```

## Approach 2: Two Pointers with Temporary Array

### Solution
typescript
```typescript
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
class Solution {
    merge(nums1: number[], m: number, nums2: number[], n: number): void {
        const temp: number[] = [];
        let p1 = 0, p2 = 0;

        // Merge elements from nums1 and nums2 into temp
        while (p1 < m && p2 < n) {
            if (nums1[p1] <= nums2[p2]) {
                temp.push(nums1[p1++]);
            } else {
                temp.push(nums2[p2++]);
            }
        }

        // Copy remaining elements from nums1
        while (p1 < m) {
            temp.push(nums1[p1++]);
        }

        // Copy remaining elements from nums2
        while (p2 < n) {
            temp.push(nums2[p2++]);
        }

        // Copy temp back to nums1
        for (let i = 0; i < temp.length; i++) {
            nums1[i] = temp[i];
        }
    }
}
```

## Approach 3: Two Pointers from End (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(m + n)
// Space Complexity: O(1)
class Solution {
    merge(nums1: number[], m: number, nums2: number[], n: number): void {
        let p1 = m - 1; // Pointer for the end of nums1's initial values
        let p2 = n - 1; // Pointer for the end of nums2
        let p = m + n - 1; // Pointer for the end of nums1's total capacity

        // Start merging from the back
        while (p1 >= 0 && p2 >= 0) {
            if (nums1[p1] > nums2[p2]) {
                nums1[p] = nums1[p1];
                p1--;
            } else {
                nums1[p] = nums2[p2];
                p2--;
            }
            p--;
        }

        // If there are leftover elements in nums2
        while (p2 >= 0) {
            nums1[p] = nums2[p2];
            p2--;
            p--;
        }
    }
}
```

