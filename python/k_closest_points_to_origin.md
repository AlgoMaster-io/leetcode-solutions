# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
python
```python
# Time Complexity: O(n * log(k)), where n is the number of points
# Space Complexity: O(k), for the max-heap
import heapq

class Solution:
    def kClosest(self, points, k):
        # Max-Heap to store the k closest points
        maxHeap = []

        # Add points to the heap and maintain only the k closest points
        for point in points:
            heapq.heappush(maxHeap, (-self.distance(point), point))
            if len(maxHeap) > k:
                heapq.heappop(maxHeap)  # Remove the farthest point

        # Extract the k closest points from the heap
        result = [heapq.heappop(maxHeap)[1] for _ in range(k)]
        
        return result

    # Helper method to calculate the squared distance from the origin
    def distance(self, point):
        return point[0] * point[0] + point[1] * point[1]
```

## Approach 2: Quickselect

### Solution
python
```python
# Time Complexity: O(n) on average, O(n^2) in the worst case
# Space Complexity: O(1), as it modifies the input array in-place
import random

class Solution:
    def kClosest(self, points, k):
        self.quickSelect(points, 0, len(points) - 1, k)
        return points[:k]

    def quickSelect(self, points, left, right, k):
        if left >= right:
            return

        pivotIndex = self.partition(points, left, right)
        if pivotIndex == k:
            return
        elif pivotIndex < k:
            self.quickSelect(points, pivotIndex + 1, right, k)
        else:
            self.quickSelect(points, left, pivotIndex - 1, k)

    def partition(self, points, left, right):
        pivot = points[right]
        pivotDistance = self.distance(pivot)
        i = left

        for j in range(left, right):
            if self.distance(points[j]) < pivotDistance:
                self.swap(points, i, j)
                i += 1
        self.swap(points, i, right)
        return i

    def swap(self, points, i, j):
        points[i], points[j] = points[j], points[i]

    def distance(self, point):
        return point[0] * point[0] + point[1] * point[1]
```

