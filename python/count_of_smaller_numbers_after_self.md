# 315. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
from typing import List

class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        result = []

        # Traverse each element and count smaller numbers to its right
        for i in range(len(nums)):
            count = 0
            for j in range(i + 1, len(nums)):
                if nums[j] < nums[i]:
                    count += 1
            result.append(count)

        return result
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from typing import List
import bisect

class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        result = []
        sortedNums = sorted(set(nums))
        fenwickTree = [0] * (len(sortedNums) + 1)
        
        # Helper function to update the Fenwick Tree
        def update(index: int, value: int):
            while index < len(fenwickTree):
                fenwickTree[index] += value
                index += index & -index

        # Helper function to query the prefix sum in the Fenwick Tree
        def query(index: int) -> int:
            sum = 0
            while index > 0:
                sum += fenwickTree[index]
                index -= index & -index
            return sum

        # Traverse nums from right to left
        for num in reversed(nums):
            rank = bisect.bisect_left(sortedNums, num) + 1
            result.append(query(rank - 1))
            update(rank, 1)

        return result[::-1]
```

## Approach 3: Merge Sort

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from typing import List

class Solution:
    def countSmaller(self, nums: List[int]) -> List[int]:
        self.counts = [0] * len(nums)
        indices = list(range(len(nums)))

        # Perform merge sort
        def mergeSort(left: int, right: int):
            if left >= right:
                return
            
            mid = (left + right) // 2
            mergeSort(left, mid)
            mergeSort(mid + 1, right)
            
            merge(left, mid, right)

        # Merge two sorted halves and update counts
        def merge(left: int, mid: int, right: int):
            tempIndices = []
            i, j = left, mid + 1
            smallerCount = 0

            # Merge the two halves
            while i <= mid and j <= right:
                if nums[indices[i]] <= nums[indices[j]]:
                    self.counts[indices[i]] += smallerCount
                    tempIndices.append(indices[i])
                    i += 1
                else:
                    smallerCount += 1
                    tempIndices.append(indices[j])
                    j += 1

            # Copy remaining elements
            while i <= mid:
                self.counts[indices[i]] += smallerCount
                tempIndices.append(indices[i])
                i += 1
            while j <= right:
                tempIndices.append(indices[j])
                j += 1

            # Copy merged result back to indices
            indices[left:right + 1] = tempIndices

        mergeSort(0, len(nums) - 1)

        # Convert counts array to list
        return self.counts
```

