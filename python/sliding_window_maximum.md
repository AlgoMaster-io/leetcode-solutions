# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(k)
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums, k):
        n = len(nums)
        result = [0] * (n - k + 1)
        deque_idx = deque()  # Stores indices of elements

        for i in range(n):
            # Remove indices out of the current window
            if deque_idx and deque_idx[0] < i - k + 1:
                deque_idx.popleft()

            # Remove elements smaller than the current element from the back
            while deque_idx and nums[deque_idx[-1]] < nums[i]:
                deque_idx.pop()

            deque_idx.append(i)  # Add the current element's index to the deque

            # Add the maximum element of the current window to the result
            if i >= k - 1:
                result[i - k + 1] = nums[deque_idx[0]]

        return result
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
python
```python
# Time Complexity: O(n log k)
# Space Complexity: O(k)
import heapq

class Solution:
    def maxSlidingWindow(self, nums, k):
        n = len(nums)
        result = [0] * (n - k + 1)
        max_heap = []  # Max-heap: (negative value, index)

        for i in range(n):
            # Add the current element to the heap
            heapq.heappush(max_heap, (-nums[i], i))

            # Remove elements that are out of the current window
            while max_heap[0][1] < i - k + 1:
                heapq.heappop(max_heap)

            # Add the maximum element of the current window to the result
            if i >= k - 1:
                result[i - k + 1] = -max_heap[0][0]

        return result
```

