# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(n)
import heapq

class Solution:
    def findMaxValueOfEquation(self, points, k):
        max_heap = []  # Max-heap: {-(y - x), x}
        max_value = float('-inf')

        for x, y in points:
            # Remove points outside the range of k
            while max_heap and x - max_heap[0][1] > k:
                heapq.heappop(max_heap)

            # If the heap is not empty, calculate the current value of the equation
            if max_heap:
                max_value = max(max_value, -max_heap[0][0] + y + x)

            # Add the current point to the heap
            heapq.heappush(max_heap, (-(y - x), x))

        return max_value
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import deque

class Solution:
    def findMaxValueOfEquation(self, points, k):
        deque_store = deque()  # Stores pairs (y - x, x)
        max_value = float('-inf')

        for x, y in points:
            # Remove points outside the range of k
            while deque_store and x - deque_store[0][1] > k:
                deque_store.popleft()

            # If the deque is not empty, calculate the current value of the equation
            if deque_store:
                max_value = max(max_value, deque_store[0][0] + y + x)

            # Remove points from the back of the deque that have a smaller y - x value
            while deque_store and deque_store[-1][0] <= y - x:
                deque_store.pop()

            # Add the current point to the deque
            deque_store.append((y - x, x))

        return max_value
```

