# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def maxResult(self, nums, k):
        deque_ = deque()  # Stores indices of potential max scores
        deque_.append(0)
        dp = [0] * len(nums)
        dp[0] = nums[0]

        for i in range(1, len(nums)):
            # Remove indices that are out of the current window
            if deque_ and deque_[0] < i - k:
                deque_.popleft()

            # Update dp[i] using the max value in the deque
            dp[i] = nums[i] + dp[deque_[0]]

            # Maintain the deque to keep indices in decreasing order of their dp values
            while deque_ and dp[deque_[-1]] <= dp[i]:
                deque_.pop()

            deque_.append(i)

        return dp[-1]
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
python
```python
# Time Complexity: O(n log k)
# Space Complexity: O(k)
import heapq

class Solution:
    def maxResult(self, nums, k):
        max_heap = [(-nums[0], 0)]  # Max-heap (Python heapq is a min-heap, so use negative values)
        score = nums[0]

        for i in range(1, len(nums)):
            # Remove elements outside the current window
            while max_heap[0][1] < i - k:
                heapq.heappop(max_heap)

            # The current score is the sum of the max score in the heap and nums[i]
            score = nums[i] - max_heap[0][0]
            heapq.heappush(max_heap, (-score, i))

        return score
```

