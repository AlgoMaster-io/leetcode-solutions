## [The Skyline Problem - LeetCode](https://leetcode.com/problems/the-skyline-problem/)

### Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sweep Line Algorithm with Max Heap](#approach-2-sweep-line-algorithm-with-max-heap)

### Approach 1: Brute Force

#### Intuition:
In this approach, for every given building, we mark its height along its span from the left edge to the right edge. Once this marking is complete, we can iterate through these markings to determine where the height changes, which will give us the skyline.

#### Solution:
1. Create an array `heights` to keep track of the maximum height at each horizontal position.
2. Iterate over each building and update the `heights` array for the building's range.
3. Traverse the `heights` array and record the position whenever the height changes.

```javascript
var getSkyline = function(buildings) {
    // Determine the maximum right edge for the skyline span
    let maxRight = 0;
    for (let b of buildings) {
        maxRight = Math.max(maxRight, b[1]);
    }

    // Initialize the heights array
    let heights = new Array(maxRight + 1).fill(0);

    // Mark heights for each building's range
    for (let b of buildings) {
        const [left, right, height] = b;
        for (let i = left; i < right; i++) {
            heights[i] = Math.max(heights[i], height);
        }
    }

    // Collect the skyline silhouette
    let skyline = [];
    let prevHeight = 0;
    for (let i = 0; i <= maxRight; i++) {
        if (heights[i] !== prevHeight) {
            skyline.push([i, heights[i]]);
            prevHeight = heights[i];
        }
    }

    return skyline;
};
```

#### Complexity Analysis:
- **Time Complexity:** O(N * W), where N is the number of buildings and W is the width spanned by the buildings.
- **Space Complexity:** O(W), as we need extra space to maintain the height array.

### Approach 2: Sweep Line Algorithm with Max Heap

#### Intuition:
This is an optimized approach using the sweep line algorithm combined with a max heap. The key idea is to process all building start and end points in sorted order and maintain a max heap of active building heights.

#### Solution:
1. For each building, generate two events: the left edge (entry) and the right edge (exit).
2. Sort these events. Entry events are prioritized over exit events at the same coordinate.
3. Use a max heap to store active building heights.
4. Traverse the events:
   - If it's an entry, add the building height to the heap.
   - If it's an exit, remove the building height from the heap.
   - Determine the current max height and add it to the result if it changes.

```javascript
class MaxHeap {
    constructor() {
        this.data = [];
    }

    insert(val) {
        this.data.push(val);
        this.bubbleUp();
    }

    remove(val) {
        const index = this.data.indexOf(val);
        if (index !== -1) {
            this.swap(index, this.data.length - 1);
            this.data.pop();
            this.bubbleDown(index);
        }
    }

    bubbleUp() {
        let index = this.data.length - 1;
        while (index > 0 && this.data[this.parent(index)] < this.data[index]) {
            this.swap(this.parent(index), index);
            index = this.parent(index);
        }
    }

    bubbleDown(index = 0) {
        const leftIndex = this.left(index);
        const rightIndex = this.right(index);
        let largest = index;

        if (leftIndex < this.data.length && this.data[leftIndex] > this.data[largest]) {
            largest = leftIndex;
        }
        
        if (rightIndex < this.data.length && this.data[rightIndex] > this.data[largest]) {
            largest = rightIndex;
        }
        
        if (largest !== index) {
            this.swap(largest, index);
            this.bubbleDown(largest);
        }
    }

    parent(index) {
        return Math.floor((index - 1) / 2);
    }

    left(index) {
        return index * 2 + 1;
    }

    right(index) {
        return index * 2 + 2;
    }

    swap(i, j) {
        [this.data[i], this.data[j]] = [this.data[j], this.data[i]];
    }

    max() {
        return this.data[0] || 0;
    }
}

var getSkyline = function(buildings) {
    const events = [];
    const result = [];
    const maxHeap = new MaxHeap();

    // Generate events
    for (let [l, r, h] of buildings) {
        events.push([l, h, 'enter']);
        events.push([r, h, 'exit']);
    }

    // Sort events
    events.sort((a, b) => a[0] - b[0] || (a[2] === 'enter' ? -1 : 1));

    for (let [pos, height, type] of events) {
        if (type === 'enter') {
            maxHeap.insert(height);
        } else {
            maxHeap.remove(height);
        }

        const currentHeight = maxHeap.max();
        if (result.length === 0 || result[result.length - 1][1] !== currentHeight) {
            result.push([pos, currentHeight]);
        }
    }

    return result;
};
```

#### Complexity Analysis:
- **Time Complexity:** O(N log N), due to sorting of events and heap operations.
- **Space Complexity:** O(N), as we store events and use a heap.

