# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
```javascript
// Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
// Space Complexity: O(k), where k is the size of the heap
function furthestBuilding(heights, bricks, ladders) {
    const minHeap = []; // Min-heap to store ladder-replaced jumps

    for (let i = 0; i < heights.length - 1; i++) {
        const climb = heights[i + 1] - heights[i];
        
        if (climb > 0) {
            minHeap.push(climb); 
            minHeap.sort((a, b) => a - b); // Maintain min-heap order

            // If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
            if (minHeap.length > ladders) {
                bricks -= minHeap.shift();
            }

            // If bricks are exhausted, return the current building index
            if (bricks < 0) {
                return i;
            }
        }
    }

    return heights.length - 1; // If all buildings are reachable
}
```

