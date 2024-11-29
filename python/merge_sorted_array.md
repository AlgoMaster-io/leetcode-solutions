# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approach 1: Using Built-in Sorting (Simplest)

### Solution
python
```python
# Time Complexity: O((m + n) log(m + n))
# Space Complexity: O(1)
class Solution:
    def merge(self, nums1, m, nums2, n):
        # Copy nums2 into nums1
        nums1[m:m+n] = nums2
        
        # Sort the combined array
        nums1.sort()
```

## Approach 2: Two Pointers with Temporary Array

### Solution
python
```python
# Time Complexity: O(m + n)
# Space Complexity: O(m + n)
class Solution:
    def merge(self, nums1, m, nums2, n):
        temp = []
        p1, p2 = 0, 0

        # Merge elements from nums1 and nums2 into temp
        while p1 < m and p2 < n:
            if nums1[p1] <= nums2[p2]:
                temp.append(nums1[p1])
                p1 += 1
            else:
                temp.append(nums2[p2])
                p2 += 1

        # Copy remaining elements from nums1
        while p1 < m:
            temp.append(nums1[p1])
            p1 += 1

        # Copy remaining elements from nums2
        while p2 < n:
            temp.append(nums2[p2])
            p2 += 1

        # Copy temp back to nums1
        nums1[:m+n] = temp
```

## Approach 3: Two Pointers from End (Optimal)

### Solution
python
```python
# Time Complexity: O(m + n)
# Space Complexity: O(1)
class Solution:
    def merge(self, nums1, m, nums2, n):
        p1, p2, p = m - 1, n - 1, m + n - 1

        # Start merging from the back
        while p1 >= 0 and p2 >= 0:
            if nums1[p1] > nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1

        # If there are leftover elements in nums2
        while p2 >= 0:
            nums1[p] = nums2[p2]
            p2 -= 1
            p -= 1
```

