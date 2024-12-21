# [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

## Table of Contents
- [Approach 1: Sorted Edges + Priority Queue](#approach-1-sorted-edges-priority-queue)

## Approach 1: Sorted Edges + Priority Queue

### Intuition:
The Skyline problem is essentially about managing the heights of buildings as you move from left to right across the plane. The key idea is to focus on the "critical points" where the height changes. These critical points are either the start or end of a building. By sorting these events and using a priority queue to track current buildings, we can determine the skyline at each step.

### Steps:
1. **Extract critical points** from the input buildings. Each building provides two points: its starting event and its end event. The start of a building contributes a positive height and the end contributes a negative height.
2. **Sort the events**: 
   - By the x-coordinate (starting position).
   - In case of ties, prioritize by height. If it's a start point, sort from high to low; if it's an end point, sort from low to high.
3. **Use a max-heap (priority queue)** to keep track of all active building heights. The maximum height in the heap represents the current tallest building that influences the skyline.
4. **Iterate through the sorted events**:
   - When adding a start event, add its height to the heap.
   - When processing an end event, remove its height from the heap.
   - Check if the current max height (top of the heap) changes before and after processing the event. If it does, add the new key point to the result.
5. **Return the list of key points** as the skyline.

### Code:
```java
import java.util.*;

public class Solution {
    public List<List<Integer>> getSkyline(int[][] buildings) {
        List<List<Integer>> result = new ArrayList<>();
        List<int[]> events = new ArrayList<>();
        
        // Step 1: Create start and end events
        for (int[] building : buildings) {
            // Start event has negative height
            events.add(new int[] { building[0], -building[2] });
            // End event has positive height
            events.add(new int[] { building[1], building[2] });
        }
        
        // Step 2: Sort events
        Collections.sort(events, (a, b) -> {
            if (a[0] != b[0]) 
                return a[0] - b[0]; // Sort by x coordinate
            return a[1] - b[1]; // In tie, sort by height
        });
        
        // Step 3: Use max-heap to manage the height
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        maxHeap.add(0); // Add a ground line height (0)
        int previousHeight = 0;
        
        // Step 4: Iterate over each event
        for (int[] event : events) {
            int x = event[0];
            int height = event[1];
            
            if (height < 0) { // Start of a building
                maxHeap.add(-height);
            } else { // End of a building
                maxHeap.remove(height);
            }
            
            int currentHeight = maxHeap.peek();
            if (currentHeight != previousHeight) {
                result.add(Arrays.asList(x, currentHeight));
                previousHeight = currentHeight;
            }
        }
        
        return result;
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(n log n), where n is the number of critical points. Sorting the events takes O(n log n), and each insertion and deletion from the heap takes O(log n).
- **Space Complexity**: O(n), as we store up to n events and heights in the heap and list.

