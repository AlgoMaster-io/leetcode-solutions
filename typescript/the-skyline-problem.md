# [Leetcode 218: The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

## Approaches
1. [Priority Queue Approach](#priority-queue-approach)
2. [Sweep Line with Sorted Multiset](#sweep-line-with-sorted-multiset)

## Priority Queue Approach

### Intuition
The Skyline Problem can be tackled using a Priority Queue (Max Heap) to track the highest building at current points along the x-axis. The main idea is to treat each building's start and end as events and process them in a sorted order. When processing, if it's a building start, it's added to the heap, and if it's an end, it's removed. The skyline changes whenever the current highest building changes.

### Approach
1. For each building represented as [Li, Ri, Hi], consider two events: start (x = Li, height = Hi) and end (x = Ri, height = Hi).
2. Sort events. Start events should come before end events if they are at the same x-coordinate. This means sorting by x, then by start/end events (start: -Hi, end: Hi).
3. Use a max-heap to track the current active buildings based on their heights.
4. Traverse through each event:
   - If it's a start event, add the building's height to the max-heap.
   - If it's an end event, remove the building's height from the heap.
5. The current height of the skyline changes if the top of the heap changes. Record these changes.

### Implementation
```typescript
class PriorityQueue {
    private heap: number[];

    constructor() {
        this.heap = [];
    }

    push(val: number) {
        this.heap.push(val);
        this.bubbleUp();
    }

    pop(): number {
        const max = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length) {
            this.heap[0] = end as number;
            this.bubbleDown();
        }
        return max;
    }

    peek(): number {
        return this.heap[0];
    }

    remove(val: number) {
        const idx = this.heap.indexOf(val);
        if (idx === -1) return;
        const end = this.heap.pop();
        if (idx !== this.heap.length) {
            this.heap[idx] = end as number;
            this.bubbleDown(idx);
            this.bubbleUp(idx);
        }
    }

    private bubbleUp(idx = this.heap.length - 1) {
        const element = this.heap[idx];
        while (idx > 0) {
            const parentIdx = Math.floor((idx - 1) / 2);
            const parent = this.heap[parentIdx];
            if (element <= parent) break;
            this.heap[parentIdx] = element;
            this.heap[idx] = parent;
            idx = parentIdx;
        }
    }

    private bubbleDown(idx = 0) {
        const length = this.heap.length;
        const element = this.heap[idx];
        while (true) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftChildIdx < length) {
                leftChild = this.heap[leftChildIdx];
                if (leftChild > element) swap = leftChildIdx;
            }
            if (rightChildIdx < length) {
                rightChild = this.heap[rightChildIdx];
                if ((swap === null && rightChild > element) || (swap !== null && rightChild > leftChild)) 
                    swap = rightChildIdx;
            }
            if (swap === null) break;
            this.heap[idx] = this.heap[swap];
            this.heap[swap] = element;
            idx = swap;
        }
    }
}

function getSkyline(buildings: number[][]): number[][] {
    // Gather all events: (x, height), where height is positive for start and negative for end
    let events = [];
    for (let [Li, Ri, Hi] of buildings) {
        events.push([Li, -Hi]);  // Start of a building
        events.push([Ri, Hi]);   // End of a building
    }

    // Sort events: primary by x coordinate, secondary by height
    events.sort((a, b) => a[0] - b[0] || a[1] - b[1]);

    const result: number[][] = [];
    const pq = new PriorityQueue();
    pq.push(0);  // Add ground level

    let prevMaxHeight = 0;

    for (let [x, h] of events) {
        if (h < 0) { // Start of a building
            pq.push(-h);
        } else { // End of a building
            pq.remove(h);
        }

        const currentMaxHeight = pq.peek();
        if (currentMaxHeight !== prevMaxHeight) {
            result.push([x, currentMaxHeight]);
            prevMaxHeight = currentMaxHeight;
        }
    }

    return result;
}
```

### Complexity
- **Time Complexity**: O(n log n), where n is the number of events. Sorting events takes O(n log n) time, and each insertion or removal operation on the heap takes O(log n).
- **Space Complexity**: O(n), to store events and the elements in the priority queue.

## Sweep Line with Sorted Multiset

### Intuition
This approach uses a clever way of maintaining the current height by using a balanced binary search tree or sorted multiset to dynamically track the heights of the buildings intersecting the sweep line. By processing all building start and end points, we update the sorted set, which immediately reflects the new skyline whenever the tallest building changes.

### Approach
1. Convert building start and end into events, where every start event increases the set and every end event decreases it.
2. Process these events in sorted order. For the same x-coordinate, process start events before end events.
3. Use a sorted data structure to keep track of current building heights.
4. Record changes to the result skyline when the max height from the sorted data structure changes.

### Implementation
```typescript
function getSkyline(buildings: number[][]): number[][] {
    let events = [];
    for (let [Li, Ri, Hi] of buildings) {
        events.push([Li, -Hi]); // building start
        events.push([Ri, Hi]);  // building end
    }

    events.sort((a, b) => a[0] !== b[0] ? a[0] - b[0] : a[1] - b[1]);

    let heightMap = [-1]; // to simulate multiset with frequency, starting with ground level
    let heightFreq = new Map([[0, 1]]);
    let result: number[][] = [];

    let curMaxHeight = 0;

    for (let [x, heightChange] of events) {
        if (heightChange < 0) {
            // Start of a building
            let height = -heightChange;
            if (!heightFreq.has(height)) {
                heightFreq.set(height, 0);
            }
            heightFreq.set(height, heightFreq.get(height)! + 1);
        } else {
            // End of a building
            let height = heightChange;
            heightFreq.set(height, heightFreq.get(height)! - 1);
            if (heightFreq.get(height) === 0) {
                heightFreq.delete(height);
            }
        }

        // Calculate current max height
        curMaxHeight = Math.max(...heightFreq.keys());

        if (result.length === 0 || result[result.length - 1][1] !== curMaxHeight) {
            result.push([x, curMaxHeight]);
        }
    }

    return result;
}
```

### Complexity
- **Time Complexity**: O(n log n), owing to event sorting and operations like insertion, deletion, and max retrieval.
- **Space Complexity**: O(n), for storing events and managing the multiset.

Through these approaches, you can tackle the Skyline Problem efficiently by employing advanced data structures like priority queues and balanced binary search trees for dynamic height tracking along a sweep line.

