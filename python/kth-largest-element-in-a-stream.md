# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Min-Heap Approach](#min-heap-approach)

## Brute Force Approach

### Intuition
For each new element added to the stream, sort the entire list and then find the kth largest element. This naive approach simply relies on the basic idea that sorting will help us find the position we're interested in.

### Code
```python
class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.stream = nums

    def add(self, val: int) -> int:
        # Add the new value to the list
        self.stream.append(val)
        # Sort the list
        self.stream.sort()
        # Return the k-th largest element which is at index -k of the sorted list
        return self.stream[-self.k]
```

### Complexity Analysis
- **Time Complexity**: 
  - `add` method: \(O(n \log n)\) where \(n\) is the number of elements in the stream, because we sort the list every time an element is added.
- **Space Complexity**: 
  - \(O(n)\) for storing the elements of the stream.

## Min-Heap Approach

### Intuition
To efficiently retrieve the kth largest element, we can use a Min-Heap. A Min-Heap of size `k` will help us keep track of the kth largest element at its root. The basic idea is to only store the largest `k` elements in the heap, with the root being the smallest among them (i.e., the kth largest in the entire list).

### Code
```python
import heapq

class KthLargest:
    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.min_heap = nums
        heapq.heapify(self.min_heap)
        # Trim the heap to the k largest elements
        while len(self.min_heap) > k:
            heapq.heappop(self.min_heap)

    def add(self, val: int) -> int:
        # If the heap has less than k elements, simply add the new value
        if len(self.min_heap) < self.k:
            heapq.heappush(self.min_heap, val)
        # If new value is greater than the root of the heap (smallest of k elements), it can change the kth largest
        elif val > self.min_heap[0]:
            heapq.heapreplace(self.min_heap, val)
        # The root of the heap is the kth largest element
        return self.min_heap[0]
```

### Complexity Analysis
- **Time Complexity**: 
  - `__init__`: \(O(n \log k)\) where \(n\) is the number of initial elements in `nums` because we might pop some elements to retain only k in the heap.
  - `add` method: \(O(\log k)\) due to the heap operations.
- **Space Complexity**: 
  - \(O(k)\), since the heap contains only `k` elements at any time.

In summary, while the brute force method is straightforward, it is inefficient with larger streams because of the sorting step. The min-heap approach, on the other hand, efficiently maintains the necessary state to deliver quick access to the kth largest element.

