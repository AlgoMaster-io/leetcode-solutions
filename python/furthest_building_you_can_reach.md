# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
python
```python
# Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
# Space Complexity: O(k), where k is the size of the heap
import heapq

class Solution:
    def furthestBuilding(self, heights, bricks, ladders):
        min_heap = []  # Min-heap to store ladder-replaced jumps

        for i in range(len(heights) - 1):
            climb = heights[i + 1] - heights[i]
            
            if climb > 0:
                heapq.heappush(min_heap, climb)  # Add the climb to the heap

                # If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
                if len(min_heap) > ladders:
                    bricks -= heapq.heappop(min_heap)

                # If bricks are exhausted, return the current building index
                if bricks < 0:
                    return i

        return len(heights) - 1  # If all buildings are reachable
```

