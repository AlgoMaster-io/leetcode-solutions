# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
csharp
```csharp
// Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
// Space Complexity: O(k), where k is the size of the heap
using System.Collections.Generic;

public class Solution {
    public int FurthestBuilding(int[] heights, int bricks, int ladders) {
        var minHeap = new SortedSet<(int, int)>();
        var index = 0;
        
        for (int i = 0; i < heights.Length - 1; i++) {
            int climb = heights[i + 1] - heights[i];
            
            if (climb > 0) {
                // Add the climb to the heap
                minHeap.Add((climb, index++)); 

                // If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
                if (minHeap.Count > ladders) {
                    bricks -= minHeap.Min.Item1;
                    minHeap.Remove(minHeap.Min);
                }

                // If bricks are exhausted, return the current building index
                if (bricks < 0) {
                    return i;
                }
            }
        }

        return heights.Length - 1; // If all buildings are reachable
    }
}
```

