# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
class Solution:
    def reversePairs(self, nums):
        if not nums or len(nums) < 2:
            return 0
        return self.mergeSort(nums, 0, len(nums) - 1)

    def mergeSort(self, nums, left, right):
        if left >= right:
            return 0

        mid = left + (right - left) // 2

        # Count reverse pairs in left and right halves, and across them
        count = self.mergeSort(nums, left, mid) + self.mergeSort(nums, mid + 1, right)

        # Count cross reverse pairs
        j = mid + 1
        for i in range(left, mid + 1):
            while j <= right and nums[i] > 2 * nums[j]:
                j += 1
            count += (j - mid - 1)

        # Merge the two halves
        self.merge(nums, left, mid, right)

        return count

    def merge(self, nums, left, mid, right):
        temp = []
        i, j = left, mid + 1

        while i <= mid and j <= right:
            if nums[i] <= nums[j]:
                temp.append(nums[i])
                i += 1
            else:
                temp.append(nums[j])
                j += 1

        while i <= mid:
            temp.append(nums[i])
            i += 1

        while j <= right:
            temp.append(nums[j])
            j += 1

        nums[left:right + 1] = temp
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
from sortedcontainers import SortedSet

class Solution:
    def reversePairs(self, nums):
        # Coordinate compression
        set = SortedSet()
        for num in nums:
            set.add(num)
            set.add(num * 2)

        map = {num: i + 1 for i, num in enumerate(set)}

        # Binary Indexed Tree
        bit = [0] * (len(map) + 1)
        count = 0

        for num in reversed(nums):
            # Count elements smaller than nums[i]
            count += self.query(bit, map[num] - 1)

            # Add nums[i] * 2 to the BIT
            self.update(bit, map[num * 2], 1)

        return count

    def update(self, bit, index, delta):
        while index < len(bit):
            bit[index] += delta
            index += index & -index

    def query(self, bit, index):
        sum = 0
        while index > 0:
            sum += bit[index]
            index -= index & -index
        return sum
```

