# [LeetCode Problem 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Solutions

1. [Naive Two-Pointer Approach](#solution-1)
2. [In-place Merge with Extra Storage](#solution-2)
3. [Optimal In-place Merge from the End](#solution-3)

---

### Solution 1: Naive Two-Pointer Approach

#### Intuition
The simplest way to merge the two sorted arrays is to use a temporary list to store the merged elements. We can use two pointers, one for each array, to traverse through them and compare elements, then store the smaller element into the temporary list.

#### Approach
- Initialize two pointers, one for `nums1` and one for `nums2`.
- Traverse both arrays, compare the current elements, and append the smaller element to an auxiliary list.
- If there are remaining elements in either array, append them to the auxiliary list.
- Copy the sorted elements from the auxiliary list back into `nums1`.

#### Code

```python
def merge(nums1, m, nums2, n):
    # Create a temporary list to hold merged elements
    merged = []
    i, j = 0, 0

    # Traverse through nums1 and nums2 and merge their elements
    while i < m and j < n:
        if nums1[i] < nums2[j]:
            merged.append(nums1[i])
            i += 1
        else:
            merged.append(nums2[j])
            j += 1

    # Append any leftover elements in nums1
    while i < m:
        merged.append(nums1[i])
        i += 1

    # Append any leftover elements in nums2
    while j < n:
        merged.append(nums2[j])
        j += 1

    # Copy elements back to nums1
    for k in range(m + n):
        nums1[k] = merged[k]

# Time Complexity: O(m + n)
# Space Complexity: O(m + n)
```

---

### Solution 2: In-place Merge with Extra Storage

#### Intuition
Instead of creating a new list, we can try to take advantage of the extra space in `nums1`. We can copy `nums1`'s elements to another list, then use the two-pointer technique to merge `nums1_copy` and `nums2` back into `nums1`.

#### Approach
- Copy the first `m` elements of `nums1` into a new list `nums1_copy`.
- Use two pointers, one for `nums1_copy` and one for `nums2`.
- Merge elements back into `nums1` by comparing elements pointed by the two pointers.

#### Code

```python
def merge(nums1, m, nums2, n):
    # Copy the elements of nums1 to a new list nums1_copy
    nums1_copy = nums1[:m]
    
    # Initialize pointers for nums1_copy and nums2
    p1, p2, k = 0, 0, 0

    # Merge nums1_copy and nums2 back into nums1
    while p1 < m and p2 < n:
        if nums1_copy[p1] < nums2[p2]:
            nums1[k] = nums1_copy[p1]
            p1 += 1
        else:
            nums1[k] = nums2[p2]
            p2 += 1
        k += 1

    # If there are remaining elements in nums1_copy
    while p1 < m:
        nums1[k] = nums1_copy[p1]
        p1 += 1
        k += 1

    # If there are remaining elements in nums2
    while p2 < n:
        nums1[k] = nums2[p2]
        p2 += 1
        k += 1

# Time Complexity: O(m + n)
# Space Complexity: O(m)
```

---

### Solution 3: Optimal In-place Merge from the End

#### Intuition
Since `nums1` has enough space to accommodate the merged array, starting from the back allows us to avoid moving elements repeatedly. We can fill `nums1` from the end towards the beginning, placing the larger of the two elements from `nums1` and `nums2` each time.

#### Approach
- Start from the last index of both `nums1` and `nums2`.
- Place the largest element in the correct position from the end.
- Continue this until all elements from `nums2` have been merged.

#### Code

```python
def merge(nums1, m, nums2, n):
    # Pointers for nums1, nums2, and the end of the merged array
    p1, p2, end = m - 1, n - 1, m + n - 1

    # While there are elements in both nums1 and nums2 to compare
    while p1 >= 0 and p2 >= 0:
        if nums1[p1] > nums2[p2]:
            nums1[end] = nums1[p1]
            p1 -= 1
        else:
            nums1[end] = nums2[p2]
            p2 -= 1
        end -= 1

    # If there are remaining elements in nums2, copy them
    while p2 >= 0:
        nums1[end] = nums2[p2]
        p2 -= 1
        end -= 1

# Time Complexity: O(m + n)
# Space Complexity: O(1)
```

This optimal approach efficiently merges the arrays in-place without requiring additional storage, achieving the task in linear time with constant extra space.

