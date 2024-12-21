# [Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches
- [Approach 1: Brute Force with Linear Search](#approach-1-brute-force-with-linear-search)
- [Approach 2: Heap Priority Queue](#approach-2-heap-priority-queue)


---

## Approach 1: Brute Force with Linear Search

### Intuition
The simplest approach to solve the problem is to iterate through each building and determine whether the jump to the next building can be achieved using available bricks and ladders. We attempt to use bricks first and only resort to ladders when bricks are insufficient. This approach ensures we use available resources optimally, although it may not be the most efficient for larger inputs.

### Implementation
```java
public class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        for (int i = 0; i < heights.length - 1; i++) {
            int diff = heights[i + 1] - heights[i];
            
            // Move to the next building if it is lower or the same height
            if (diff <= 0) continue;
            
            // Use bricks if the next building is higher
            if (diff <= bricks) {
                bricks -= diff;
            } 
            // Use a ladder if the bricks aren't enough
            else if (ladders > 0) {
                ladders--;
            } 
            // Return the index if neither bricks nor ladders are sufficient
            else {
                return i;
            }
        }
        return heights.length - 1;
    }
}
```

### Time Complexity
- The time complexity is O(n), where n is the number of buildings.
- We inspect each pair of buildings once.

### Space Complexity
- The space complexity is O(1), as we use constant extra space.


---

## Approach 2: Heap Priority Queue

### Intuition
The Brute Force Method involves using resources optimally one at a time, but an optimal approach would be to strategically use ladders and conserve bricks for smaller gaps. This can be managed better by utilizing a min-heap (priority queue), focusing on the largest height differences where ladders are more beneficial.

1. Always attempt to use bricks for the jump.
2. If bricks run out, swap the largest brick usage with a ladder using the heap to track the largest jumps requiring bricks.
3. If the swap exhausts the ladders, return the index.

### Implementation
```java
import java.util.PriorityQueue;

public class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        
        for (int i = 0; i < heights.length - 1; i++) {
            int diff = heights[i + 1] - heights[i];
            
            // Only consider jumps that require climbing up
            if (diff > 0) {
                pq.add(diff);  // Track this jump in the heap
                
                // If bricks are insufficient, replace the largest used bricks with a ladder
                if (pq.size() > ladders) {
                    bricks -= pq.poll();  // Remove the smallest jump once ladders replace enough bricks
                }
                
                // If bricks run out, return the current position
                if (bricks < 0) {
                    return i;
                }
            }
        }
        return heights.length - 1;
    }
}
```

### Time Complexity
- The time complexity is O(n log k), where n is the number of buildings and k is the number of ladders, due to heap operations (insertion and deletion).

### Space Complexity
- The space complexity is O(k), where k is the number of ladders, corresponding to the maximum size of the heap.

