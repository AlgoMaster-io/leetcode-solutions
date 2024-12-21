Sure! Below is the markdown format with solutions for the Leetcode problem "1642: Furthest Building You Can Reach".

---

## [Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

### Approaches:
- [Approach 1: Greedy + Sorting (Basic)](#approach-1-greedy--sorting-basic)
- [Approach 2: Greedy + Min-Heap/Priority Queue (Optimal)](#approach-2-greedy--min-heappriority-queue-optimal)

---

### Approach 1: Greedy + Sorting (Basic)

**Intuition:**

The basic idea is to perform a greedy strategy by attempting to use ladders on the largest jumps first. For smaller jumps, we can use bricks. This can be achieved by sorting the jumps in descending order and first using ladders on the largest possible jumps.

**Steps:**

1. Calculate all the jumps needed between consecutive buildings.
2. Sort the jumps in descending order.
3. Use ladders on the largest jumps, and for the remaining, use bricks.
4. Keep track of bricks, and if they're exhausted, track how many buildings can be reached.

**Code:**

```javascript
function furthestBuilding(heights, bricks, ladders) {
    const n = heights.length;
    const jumps = [];

    // Calculate jumps
    for (let i = 0; i < n - 1; i++) {
        const jump = heights[i + 1] - heights[i];
        if (jump > 0) {
            jumps.push([jump, i]);
        }
    }

    // Sort jumps in descending order by height difference
    jumps.sort((a, b) => b[0] - a[0]);

    let buildingsReached = 0;
    for (let i = 0; i < n - 1; i++) {
        const jump = heights[i + 1] - heights[i];

        if (jump > 0 && bricks >= jump) {
            bricks -= jump;
            buildingsReached = i + 1;
        } else if (ladders > 0) {
            ladders--;
            buildingsReached = i + 1;
        } else {
            break;
        }
    }
    return buildingsReached;
}
```

**Complexity:**

- Time Complexity: O(n log n) due to sorting the jumps array.
- Space Complexity: O(n) for storing the jumps array.

---

### Approach 2: Greedy + Min-Heap/Priority Queue (Optimal)

**Intuition:**

The optimal approach utilizes a priority queue (min-heap) to keep track of the largest jumps we decide to use bricks on. This allows us to efficiently switch from using bricks to ladders when necessary. By leveraging a min-heap, we prioritize the smallest brick usages to replace by ladders when we run out of bricks.

**Steps:**

1. Use a min-heap to keep track of the largest used brick jumps.
2. Traverse through the buildings, and for each positive jump:
   - Add the jump to the heap.
   - If the heap's size exceeds the number of ladders available, use bricks for the smallest element in the heap (the largest for which bricks are currently in use).
   - If bricks are exhausted during this process, stop and return the current building index.
3. Continue until ladders and bricks resourcearily become impractical.

**Code:**

```javascript
function furthestBuilding(heights, bricks, ladders) {
    const minHeap = new MinPriorityQueue();

    for (let i = 0; i < heights.length - 1; i++) {
        const jump = heights[i + 1] - heights[i];

        if (jump > 0) {
            minHeap.enqueue(jump);
        }

        // If the heap has more elements than ladders
        if (minHeap.size() > ladders) {
            bricks -= minHeap.dequeue().element;

            // If bricks run out, return current building index
            if (bricks < 0) {
                return i;
            }
        }
    }
    return heights.length - 1;
}
```

**Complexity:**

- Time Complexity: O(n log k), where `k` is the number of ladders, because we insert into and remove from the heap typically up to k times.
- Space Complexity: O(k) for maintaining the heap.

---

This concludes the solutions for the problem using different approaches. Approach 2 provides a more efficient way of solving the problem using a priority queue to balance the use of bricks and ladders effectively.

