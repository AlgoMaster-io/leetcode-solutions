# [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Priority Queue (Max-Heap)](#approach-2-priority-queue-max-heap)

---

### Approach 1: Brute Force

**Intuition**

The simplest way to solve this problem is to iterate through each building and decide whether to use bricks, a ladder, or if it's simply not possible to move to the next building. This approach checks every possible step and uses resources naively without trying to optimize their usage.

**Steps:**
1. Traverse the buildings from left to right.
2. For each building, calculate the height difference between the current building and the next one.
3. If the difference is less than or equal to zero, continue to the next building.
4. If the difference is positive, use bricks if available.
5. If bricks are insufficient, attempt to use a ladder.
6. If neither ladders nor sufficient bricks are available, return the index of the current building as the furthest reachable building.

**Code in TypeScript:**

```typescript
function furthestBuilding(heights: number[], bricks: number, ladders: number): number {
    for (let i = 0; i < heights.length - 1; i++) {
        const climb = heights[i + 1] - heights[i];
        
        if (climb <= 0) {
            continue; // No need for bricks or ladders
        }
        
        if (bricks >= climb) {
            bricks -= climb; // Use bricks if available
        } else if (ladders > 0) {
            ladders--; // Use ladder if bricks are not enough
        } else {
            return i; // Return the current index as the furthest
        }
    }
    
    return heights.length - 1; // Reach the end if possible
}
```

**Complexity Analysis**

- **Time Complexity:** O(n), where n is the number of buildings. We are processing each building once.
- **Space Complexity:** O(1), no additional data structures are used.

---

### Approach 2: Priority Queue (Max-Heap)

**Intuition**

In an optimal solution, the crux of the problem is managing when to use bricks and when to use ladders. A good strategy involves always using a ladder for the largest climbovercome so far, thereby saving bricks for smaller climbs. We can use a max-heap (priority queue) to keep track of the climbs we've encountered so far, and efficiently decide when to substitute bricks with a ladder.

**Steps:**
1. Traverse through the buildings.
2. Calculate the difference in height to the next building.
3. If the difference is positive, push it onto a max-heap.
4. If the size of the heap exceeds the number of ladders, pop the largest climb and use bricks.
5. If at any point using bricks results in a negative number of bricks, return the current index as the furthest reachable building.

**Code in TypeScript:**

```typescript
import { MaxHeap } from "typescript-collections";

function furthestBuilding(heights: number[], bricks: number, ladders: number): number {
    const ladderAllocations = new MaxHeap<number>();

    for (let i = 0; i < heights.length - 1; i++) {
        const climb = heights[i + 1] - heights[i];

        if (climb > 0) {
            ladderAllocations.add(climb);

            if (ladderAllocations.size() > ladders) {
                const bricksNeeded = ladderAllocations.poll()!;
                bricks -= bricksNeeded;

                if (bricks < 0) {
                    return i; // Return the current index as the furthest
                }
            }
        }
    }

    return heights.length - 1; // Reach the last building if possible
}
```

**Complexity Analysis**

- **Time Complexity:** O(n log k), where n is the number of buildings and k is the number of ladders. Each heap operation (add or poll) takes O(log k) time.
- **Space Complexity:** O(k), where k is the number of ladders - as this is the maximum size of the heap.

