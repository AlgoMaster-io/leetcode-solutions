# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
```cpp
// Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
// Space Complexity: O(k), where k is the size of the heap
#include <queue>
#include <vector>

class Solution {
public:
    int furthestBuilding(std::vector<int>& heights, int bricks, int ladders) {
        std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // Min-heap to store ladder-replaced jumps

        for (int i = 0; i < heights.size() - 1; i++) {
            int climb = heights[i + 1] - heights[i];
            
            if (climb > 0) {
                minHeap.push(climb); // Add the climb to the heap

                // If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
                if (minHeap.size() > ladders) {
                    bricks -= minHeap.top();
                    minHeap.pop();
                }

                // If bricks are exhausted, return the current building index
                if (bricks < 0) {
                    return i;
                }
            }
        }

        return heights.size() - 1; // If all buildings are reachable
    }
};
```

