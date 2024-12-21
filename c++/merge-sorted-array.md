Sure! Let's dive into the Leetcode problem "Merge Sorted Array".

### [Leetcode Problem 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into `nums1` as one sorted array.

- The number of elements initialized in `nums1` and `nums2` are `m` and `n` respectively.
- You may assume that `nums1` has enough space (size that is equal to `m + n`) to hold additional elements from `nums2`.

### Table of Contents

1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Two Pointers (Merge Approach)](#approach-2-two-pointers-merge-approach)
3. [Approach 3: Optimized Two Pointers (Start from the end)](#approach-3-optimized-two-pointers-start-from-the-end)

---

### Approach 1: Brute Force

Naively, we can use a brute force method where we append all elements of `nums2` to `nums1`, then sort the resulting array.

#### Intuition:
1. Since `nums1` has enough space, append all elements of `nums2` into the end of `nums1`.
2. Sort the entire `nums1` array.

This approach will not utilize the pre-sorted nature of the arrays effectively.

#### C++ Code:
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // Copy elements of nums2 to nums1 starting at index m
        for(int i = 0; i < n; ++i) {
            nums1[m + i] = nums2[i];
        }
        // Sort the entire nums1 array
        sort(nums1.begin(), nums1.begin() + m + n);
    }
};
```

#### Time Complexity:
- **O((m + n) log(m + n))**: Due to the sort operation on an array of size `m + n`.

#### Space Complexity:
- **O(1)**: No additional space is used other than modifying `nums1` in place.

---

### Approach 2: Two Pointers (Merge Approach)

We can use a more efficient approach by taking advantage of the fact that both arrays are sorted. Use two pointers to iterate through both arrays and merge them.

#### Intuition:
1. Use two pointers, one starting at the beginning of `nums1` and one at the beginning of `nums2`.
2. Create a new array to store the results temporarily.
3. Compare elements pointed by both pointers and put the smaller one in the new array.
4. Continue until all elements are processed.

#### C++ Code:
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // Create a copy of nums1
        vector<int> nums1_copy = nums1;
        int p1 = 0, p2 = 0;
        
        // Start current at the beginning of nums1 for overwriting
        for (int current = 0; current < m + n; ++current) {
            if (p2 >= n || (p1 < m && nums1_copy[p1] <= nums2[p2])) {
                nums1[current] = nums1_copy[p1++];
            } else {
                nums1[current] = nums2[p2++];
            }
        }
    }
};
```

#### Time Complexity:
- **O(m + n)**: Each element from both the arrays is processed once.

#### Space Complexity:
- **O(m)**: Used a copy of `nums1`.

---

### Approach 3: Optimized Two Pointers (Start from the end)

This approach is a more optimal version utilizing a similar two-pointers technique but merging starting from the end of `nums1` where extra space is available.

#### Intuition:
1. Use pointers at the end of `m` in `nums1` and at the end `n` in `nums2`.
2. Compare elements from end to start and fill `nums1` from the back.
3. This way, you eliminate the need of an extra array to store results.

#### C++ Code:
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = m - 1; // Pointer for nums1
        int p2 = n - 1; // Pointer for nums2
        int p = m + n - 1; // The last index of merged array
        
        // Fill nums1 starting from the end
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
        
        // If any elements left in nums2
        while (p2 >= 0) {
            nums1[p] = nums2[p2];
            p2--;
            p--;
        }
    }
};
```

#### Time Complexity:
- **O(m + n)**: Each element from both the arrays is processed once.

#### Space Complexity:
- **O(1)**: Merging in place without using additional space.

