# [Leetcode 1499: Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Priority Queue (Heap)](#approach-2-priority-queue-heap)
- [Approach 3: Monotonic Deque (Optimal)](#approach-3-monotonic-deque-optimal)

---

### Approach 1: Brute Force

**Intuition:**

In this approach, we will iterate over all pairs of points `(x_i, y_i)` and `(x_j, y_j)` where `j > i` and `x_j - x_i <= k`. We will calculate and track the maximum value of `y_i + y_j + |x_j - x_i|`. 

This approach is very straightforward but inefficient due to the nested iteration over points.

**Time Complexity:** O(n^2), where n is the number of points. This is because for each point, we iterate over all subsequent points to find valid pairs.

**Space Complexity:** O(1), since no additional space is used.

```python
def findMaxValueOfEquation(points, k):
    max_val = float('-inf')
    n = len(points)
    
    for i in range(n):
        for j in range(i + 1, n):
            if points[j][0] - points[i][0] <= k:
                # Calculate the value of the equation
                value = points[i][1] + points[j][1] + points[j][0] - points[i][0]
                # Update the maximum value
                max_val = max(max_val, value)
    
    return max_val
```

---

### Approach 2: Priority Queue (Heap)

**Intuition:**

By leveraging a priority queue (max heap), we can efficiently track the maximum possible value of `y_i - x_i` for any valid `(x_i, y_i)`. 

For each point `(x_j, y_j)`, we find and maintain past points `(x_i, y_i)` in a heap where `x_j - x_i <= k`. The value we are interested in at each step becomes `y_j + x_j + (y_i - x_i)`, where `(y_i - x_i)` is the top value in our max heap.

**Time Complexity:** O(nlogn), due to potentially pushing and popping elements in the priority queue for each point.

**Space Complexity:** O(n), where n is the space used by the heap.

```python
import heapq

def findMaxValueOfEquation(points, k):
    max_val = float('-inf')
    heap = []  # max heap to store (-yi + xi, xi) tuples

    for xj, yj in points:
        # Remove points from the heap where xj - xi > k
        while heap and xj - heap[0][1] > k:
            heapq.heappop(heap)
            
        if heap:
            # Calculate and update the max_val with current yj + xj
            max_val = max(max_val, yj + xj - heap[0][0])

        # Push current point into heap
        heapq.heappush(heap, (-(yj - xj), xj))
    
    return max_val
```

---

### Approach 3: Monotonic Deque (Optimal)

**Intuition:**

Utilize a deque to maintain a decreasing order of values `y_i - x_i` while ensuring the constraints `x_j - x_i <= k` between points are satisfied. This approach allows constant time retrieval and removal of the best candidate for each point `(x_j, y_j)`.

Each point is added or removed from the deque at most once, keeping operations efficient.

**Time Complexity:** O(n), since each point is added to and removed from the deque only once.

**Space Complexity:** O(n), where n is the space used by the deque.

```python
from collections import deque

def findMaxValueOfEquation(points, k):
    max_val = float('-inf')
    dq = deque()  # store pairs of (yi - xi, xi)

    for xj, yj in points:
        # Remove points from the deque where xj - xi > k
        while dq and xj - dq[0][1] > k:
            dq.popleft()

        if dq:
            # Calculate and update the max_val with current yj + xj
            max_val = max(max_val, yj + xj + dq[0][0])

        # Maintain decreasing order in deque, remove smaller values
        while dq and dq[-1][0] <= yj - xj:
            dq.pop()
        
        # Append current point to the deque
        dq.append((yj - xj, xj))
    
    return max_val
```

The deque-based approach is the optimal solution due to its linear time complexity, which provides a significant performance improvement for large datasets.

