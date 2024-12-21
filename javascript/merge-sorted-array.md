# [Leetcode 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approaches

1. [Brute Force: Sort Joined Arrays](#approach-1-brute-force-sort-joined-arrays)
2. [Two Pointers: Fill and Sort Empty Spaces in `nums1`](#approach-2-two-pointers-fill-and-sort-empty-spaces)
3. [Optimal: Two Pointers From the End](#approach-3-optimal-two-pointers-from-the-end)

---

## Approach 1: Brute Force: Sort Joined Arrays

### Intuition

The simplest solution is to merge the two arrays by adding elements from `nums2` into `nums1` and then sorting the final array. This works because we are guaranteed that the result will fit into `nums1` due to its initial buffer of zeros.

### Code

```javascript
var merge = function(nums1, m, nums2, n) {
    // Copy all elements from nums2 to nums1 starting from index m
    for (let i = 0; i < n; i++) {
        nums1[m + i] = nums2[i];
    }
    // Sort the array nums1
    nums1.sort((a, b) => a - b);
};
```

### Complexity Analysis

- **Time Complexity**: O((m+n) log(m+n)) because we sort the combined array of m+n elements.
- **Space Complexity**: O(1) because the sorting is done in-place.

---

## Approach 2: Two Pointers: Fill and Sort Empty Spaces in `nums1`

### Intuition

Given that both arrays are sorted, we can use two pointers to compare elements in `nums1` and `nums2`, inserting the smallest one in the next available position of a new array, and thus building a sorted merged array without additional sorting.

### Code

```javascript
var merge = function(nums1, m, nums2, n) {
    // Create a new array to hold the merged result
    let merged = [];
    let i = 0; // Pointer for nums1
    let j = 0; // Pointer for nums2

    // While there are elements in both nums1 and nums2
    while (i < m && j < n) {
        // Compare and add the smallest to merged
        if (nums1[i] < nums2[j]) {
            merged.push(nums1[i]);
            i++;
        } else {
            merged.push(nums2[j]);
            j++;
        }
    }

    // If there are remaining elements in nums1
    while (i < m) {
        merged.push(nums1[i]);
        i++;
    }

    // If there are remaining elements in nums2
    while (j < n) {
        merged.push(nums2[j]);
        j++;
    }

    // Copy merged back to nums1
    for (let k = 0; k < m + n; k++) {
        nums1[k] = merged[k];
    }
};
```

### Complexity Analysis

- **Time Complexity**: O(m + n) because we traverse both arrays once.
- **Space Complexity**: O(m + n) due to creation of the new array to hold the merged result.

---

## Approach 3: Optimal: Two Pointers From the End

### Intuition

We can reduce space usage by filling `nums1` from the end backwards. This avoids needing an additional array and uses the unused elements in `nums1` to store elements from `nums2`.

### Code

```javascript
var merge = function(nums1, m, nums2, n) {
    // Last index to place the largest element in nums1
    let last = m + n - 1;

    // Start from the end of both arrays
    let i = m - 1;
    let j = n - 1;

    // Compare elements from the back, and fill nums1 from the last index
    while (j >= 0) {
        // If nums1's element is larger, place it at the end of nums1
        if (i >= 0 && nums1[i] > nums2[j]) {
            nums1[last] = nums1[i];
            i--;
        } else {  // Otherwise place nums2's element
            nums1[last] = nums2[j];
            j--;
        }
        last--; // Move the last index backward
    }
};
```

### Complexity Analysis

- **Time Complexity**: O(m + n) because we traverse each element only once.
- **Space Complexity**: O(1) as merge is done in-place without additional arrays.

---

