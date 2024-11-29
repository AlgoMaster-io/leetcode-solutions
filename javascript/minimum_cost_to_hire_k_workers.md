# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
```javascript
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap

function mincostToHireWorkers(quality, wage, k) {
    const n = quality.length;
    const workers = Array.from({ length: n }, (_, i) => [wage[i] / quality[i], quality[i]]);

    // Sort workers by their wage-to-quality ratio
    workers.sort((a, b) => a[0] - b[0]);

    const maxHeap = new MaxHeap(); // Custom Max-Heap implementation
    let totalQuality = 0; // Sum of qualities in the current group
    let minCost = Infinity;

    for (const [ratio, currentQuality] of workers) {
        totalQuality += currentQuality;
        maxHeap.add(currentQuality);

        // If we exceed k workers, remove the one with the highest quality
        if (maxHeap.size() > k) {
            totalQuality -= maxHeap.poll();
        }

        // If we have exactly k workers, calculate the cost
        if (maxHeap.size() === k) {
            minCost = Math.min(minCost, totalQuality * ratio);
        }
    }

    return minCost;
}

class MaxHeap {
    constructor() {
        this.heap = [];
    }

    add(val) {
        this.heap.push(val);
        this._heapifyUp();
    }

    poll() {
        const max = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = end;
            this._heapifyDown();
        }
        return max;
    }

    size() {
        return this.heap.length;
    }

    _heapifyUp() {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] <= this.heap[parentIndex]) break;
            [this.heap[index], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[index]];
            index = parentIndex;
        }
    }

    _heapifyDown() {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[0];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swapIndex = null;

            if (leftChildIndex < length) {
                if (this.heap[leftChildIndex] > element) {
                    swapIndex = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                if (
                    (swapIndex === null && this.heap[rightChildIndex] > element) ||
                    (swapIndex !== null && this.heap[rightChildIndex] > this.heap[leftChildIndex])
                ) {
                    swapIndex = rightChildIndex;
                }
            }

            if (swapIndex === null) break;

            [this.heap[index], this.heap[swapIndex]] = [this.heap[swapIndex], this.heap[index]];
            index = swapIndex;
        }
    }
}
```

