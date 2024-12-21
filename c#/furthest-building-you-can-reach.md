[Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Min-Heap (Priority Queue) Approach (Optimal)](#min-heap-priority-queue-approach-optimal)

### Brute Force Approach

#### Intuition
The simplest approach is to iterate through the building heights. For each transition from one building to the next, check if the height increases, and consume bricks or a ladder accordingly. Use ladders first for larger height differences and reserve bricks for smaller climbs. Iterate until you run out of resources.

#### Code
```csharp
public int FurthestBuilding(int[] heights, int bricks, int ladders) {
    for (int i = 0; i < heights.Length - 1; i++) {
        int climb = heights[i + 1] - heights[i];
        
        // If there's no climb needed
        if (climb <= 0) continue;
        
        // If climb is needed, first prefer using bricks if available
        if (climb <= bricks) {
            bricks -= climb;
        } else if (ladders > 0) {
            // If not enough bricks, try using a ladder
            ladders--;
        } else {
            // If neither bricks nor ladders can handle this climb, return current position
            return i;
        }
    }
    
    // If we never run out of resources, we can reach the last building
    return heights.Length - 1;
}
```

#### Time Complexity
- **O(N)**: We traverse each building once.
#### Space Complexity
- **O(1)**: The space usage doesn't depend on the input size.

### Min-Heap (Priority Queue) Approach (Optimal)

#### Intuition
To optimally decide whether to use bricks or ladders for each step, we can leverage a min-heap (priority queue). The idea is to use ladders for the largest climbs while using bricks for smaller ones. As we track climbs, we keep the largest climbers in the heap and choose to replace smaller climbs with a ladder when we run out of bricks.

Using a priority queue helps manage and replace the largest elements efficiently.

#### Code
```csharp
using System.Collections.Generic;

public int FurthestBuilding(int[] heights, int bricks, int ladders) {
    // Min-heap to store the used bricks on climbs
    PriorityQueue<int, int> pq = new PriorityQueue<int, int>();
    
    for (int i = 0; i < heights.Length - 1; i++) {
        int climb = heights[i + 1] - heights[i];
        
        // If no climb or downward movement, continue
        if (climb <= 0) continue;
        
        // Enqueue the climb because we have to climb it
        pq.Enqueue(climb, climb);
        
        // Use bricks for the smallest climb
        bricks -= climb;
        
        // If bricks go negative, replace the smallest climb brick usage with ladder
        if (bricks < 0) {
            if (ladders > 0) {
                bricks += pq.Dequeue();
                ladders--;
            } else {
                // If no ladders and bricks left, return current index
                return i;
            }
        }
    }
    
    // If we manage to process all buildings
    return heights.Length - 1;
}
```

#### Time Complexity
- **O(N log L)**: Where N is the number of buildings and L is the number of ladders. Each addition and removal operation to the priority queue costs O(log L).
#### Space Complexity
- **O(L)**: The space complexity depends on the number of ladders as it limits the size of the priority queue.

