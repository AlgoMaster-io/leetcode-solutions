# [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches
- [Approach 1: Sort-all approach](#approach-1-sort-all-approach)
- [Approach 2: Quickselect](#approach-2-quickselect)
- [Approach 3: Min Heap](#approach-3-min-heap)

## Approach 1: Sort-all approach

### Intuition
The simplest solution is to calculate the Euclidean distance for each point from the origin and sort them purely based on this distance. The first `K` points in the sorted array will be the answer since they are the closest points.

### Steps
1. Calculate the Euclidean distance from the origin for each point. The squared distance suffices for comparison, i.e., \( x^2 + y^2 \).
2. Sort all the points based on the computed squared distance.
3. Return the first `K` points from the sorted list.

### Code
```python
def kClosest(points, K):
    # Sort the points based on their squared distance from origin
    points.sort(key=lambda point: point[0] ** 2 + point[1] ** 2)
    # Return the first K points from the sorted list
    return points[:K]
```

### Time Complexity
- Sorting the array takes \(O(N \log N)\), where \(N\) is the number of points.
- Retrieving the first `K` points takes \(O(K)\).
  
Overall time complexity is: \(O(N \log N)\).

### Space Complexity
- The space required is \(O(1)\) if we do not count the input list and output list storage.

## Approach 2: Quickselect

### Intuition
Quickselect is a selection algorithm to find the `K`-th smallest element in an unordered list. It's related to the quicksort algorithm. The advantage of quickselect is that its time complexity is better than a full sort when we only need the `K` smallest elements.

### Steps
1. Select a pivot point, which, in a compact implementation, is usually the last element.
2. Partition the elements so that elements less than the pivot come before it, and elements greater or equal come after it.
3. If the pivot ends up in position `K`, we are done.
4. Otherwise, recurse into the correct partition.

### Code
```python
import random

def kClosest(points, K):
    def distance(point):
        # Squared distance from the origin
        return point[0] ** 2 + point[1] ** 2

    def quickselect(left, right, K):
        if left < right:
            pivot_index = partition(left, right)
            # Number of elements in the left partition
            if pivot_index == K:
                return
            elif pivot_index < K:
                quickselect(pivot_index + 1, right, K)
            else:
                quickselect(left, pivot_index - 1, K)
    
    def partition(left, right):
        pivot_index = random.randint(left, right)
        pivot_value = distance(points[pivot_index])
        points[pivot_index], points[right] = points[right], points[pivot_index]
        
        store_index = left
        for i in range(left, right):
            if distance(points[i]) < pivot_value:
                points[i], points[store_index] = points[store_index], points[i]
                store_index += 1
        points[store_index], points[right] = points[right], points[store_index]
        return store_index

    quickselect(0, len(points) - 1, K)
    return points[:K]
```

### Time Complexity
- The average time complexity of Quickselect is \(O(N)\) due to the selective recursive partitioning.

### Space Complexity
- The space complexity is \(O(1)\) for the iterative approach.

## Approach 3: Min Heap

### Intuition
A Min Heap is useful to always extract the minimum element. By creating a max heap of size `K`, we can always efficiently keep track of the `K` closest points.

### Steps
1. Use a heap of size `K` to store the points.
2. As you iterate over the points, if the current point is closer than the farthest point in the heap, replace it.
3. The points remaining in the heap after processing all points will be the `K` closest ones.

### Code
```python
import heapq

def kClosest(points, K):
    # Create a max heap
    heap = []
    for point in points:
        # Calculate squared distance
        dist = -(point[0] ** 2 + point[1] ** 2)
        if len(heap) < K:
            # Heap size is less than K, directly push the item
            heapq.heappush(heap, (dist, point))
        else:
            # Push to heap if distance is larger and pop the max
            heapq.heappushpop(heap, (dist, point))
    
    # Return results from the heap
    return [point for (_, point) in heap]
```

### Time Complexity
- Building the heap of the K elements takes \(O(N \log K)\).

### Space Complexity
- The space complexity is \(O(K)\) for the heap.

