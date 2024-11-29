# 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach 1: Brute Force (Merge and Find Median)

### Solution
python
```python
# Time Complexity: O(m + n)
# Space Complexity: O(m + n)
class Solution:
    def findMedianSortedArrays(self, nums1: list[int], nums2: list[int]) -> float:
        m, n = len(nums1), len(nums2)
        merged = []

        # Merge the two arrays into one sorted array
        i = j = 0
        while i < m and j < n:
            if nums1[i] < nums2[j]:
                merged.append(nums1[i])
                i += 1
            else:
                merged.append(nums2[j])
                j += 1

        # Add remaining elements from nums1
        while i < m:
            merged.append(nums1[i])
            i += 1

        # Add remaining elements from nums2
        while j < n:
            merged.append(nums2[j])
            j += 1

        # Find and return the median
        total_length = m + n
        if total_length % 2 == 0:
            return (merged[total_length // 2 - 1] + merged[total_length // 2]) / 2.0
        else:
            return merged[total_length // 2]
```

## Approach 2: Binary Search on Partition Index (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(log(min(m, n)))
# Space Complexity: O(1)
class Solution:
    def findMedianSortedArrays(self, nums1: list[int], nums2: list[int]) -> float:
        if len(nums1) > len(nums2):
            # Ensure nums1 is the smaller array
            nums1, nums2 = nums2, nums1

        m, n = len(nums1), len(nums2)
        start, end = 0, m

        while start <= end:
            partitionX = start + (end - start) // 2
            partitionY = (m + n + 1) // 2 - partitionX

            # Edge cases: Use float('-inf') and float('inf') for out-of-bound partitions
            maxX = float('-inf') if partitionX == 0 else nums1[partitionX - 1]
            minX = float('inf') if partitionX == m else nums1[partitionX]
            maxY = float('-inf') if partitionY == 0 else nums2[partitionY - 1]
            minY = float('inf') if partitionY == n else nums2[partitionY]

            if maxX <= minY and maxY <= minX:
                # Correct partition found
                if (m + n) % 2 == 0:
                    # Even total length: Average of max of left and min of right
                    return (max(maxX, maxY) + min(minX, minY)) / 2.0
                else:
                    # Odd total length: Max of left partition
                    return max(maxX, maxY)
            elif maxX > minY:
                # Move left in nums1
                end = partitionX - 1
            else:
                # Move right in nums1
                start = partitionX + 1

        raise ValueError("Input arrays are not sorted or invalid.")
```

