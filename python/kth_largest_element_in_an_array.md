# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1)
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()  # Sort the array in ascending order
        return nums[-k]  # Return the kth largest element
```

## Approach 2: Using a Min-Heap

### Solution
```python
# Time Complexity: O(n log k)
# Space Complexity: O(k)
import heapq

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        minHeap = []  # Min-heap to keep the top k elements

        for num in nums:
            heapq.heappush(minHeap, num)  # Add the current number to the heap
            if len(minHeap) > k:
                heapq.heappop(minHeap)  # Remove the smallest element if the heap size exceeds k

        return minHeap[0]  # The root of the heap is the kth largest element
```

## Approach 3: Quickselect (Partitioning)

### Solution
```python
# Time Complexity: O(n) on average, O(n^2) in the worst case
# Space Complexity: O(1)
from random import randint

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        n = len(nums)
        return self.quickSelect(nums, 0, n - 1, n - k)

    def quickSelect(self, nums: List[int], left: int, right: int, k: int) -> int:
        if left == right:
            return nums[left]  # Base case: only one element

        pivotIndex = left + randint(0, right - left)

        pivotIndex = self.partition(nums, left, right, pivotIndex)

        if pivotIndex == k:
            return nums[k]  # Found the kth largest element
        elif pivotIndex < k:
            return self.quickSelect(nums, pivotIndex + 1, right, k)  # Search in the right part
        else:
            return self.quickSelect(nums, left, pivotIndex - 1, k)  # Search in the left part

    def partition(self, nums: List[int], left: int, right: int, pivotIndex: int) -> int:
        pivotValue = nums[pivotIndex]
        self.swap(nums, pivotIndex, right)  # Move pivot to the end
        storeIndex = left

        for i in range(left, right):
            if nums[i] < pivotValue:
                self.swap(nums, storeIndex, i)
                storeIndex += 1

        self.swap(nums, storeIndex, right)  # Move pivot to its final position
        return storeIndex

    def swap(self, nums: List[int], i: int, j: int) -> None:
        nums[i], nums[j] = nums[j], nums[i]
```

