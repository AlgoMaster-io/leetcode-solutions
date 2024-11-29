# 1642. [Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approach 1: Max-Heap to Track Largest Jumps

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)), where n is the number of buildings and k is the number of jumps stored in the heap
// Space Complexity: O(k), where k is the size of the heap
function furthestBuilding(heights: number[], bricks: number, ladders: number): number {
    const minHeap = new MinHeap<number>(); // Min-heap to store ladder-replaced jumps

    for (let i = 0; i < heights.length - 1; i++) {
        const climb = heights[i + 1] - heights[i];
        
        if (climb > 0) {
            minHeap.add(climb); // Add the climb to the heap

            // If the number of climbs exceeds the number of ladders, use bricks for the smallest jump
            if (minHeap.size() > ladders) {
                bricks -= minHeap.poll();
            }

            // If bricks are exhausted, return the current building index
            if (bricks < 0) {
                return i;
            }
        }
    }

    return heights.length - 1; // If all buildings are reachable
}

// Helper min-heap class
class MinHeap<T> {
    private heap: T[] = [];

    add(value: T): void {
        this.heap.push(value);
        this.bubbleUp();
    }

    poll(): T | undefined {
        if (this.size() === 0) return undefined;

        const minValue = this.heap[0];
        const end = this.heap.pop();

        if (this.size() > 0 && end !== undefined) {
            this.heap[0] = end;
            this.bubbleDown();
        }

        return minValue;
    }

    size(): number {
        return this.heap.length;
    }

    private bubbleUp(): void {
        let index = this.heap.length - 1;
        const element = this.heap[index];

        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];

            if (element >= parent) break;

            this.heap[index] = parent;
            index = parentIndex;
        }

        this.heap[index] = element;
    }

    private bubbleDown(): void {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[0];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swap = null;

            if (leftChildIndex < length) {
                const leftChild = this.heap[leftChildIndex];
                if (leftChild < element) {
                    swap = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                const rightChild = this.heap[rightChildIndex];
                if (
                    (swap === null && rightChild < element) ||
                    (swap !== null && rightChild < this.heap[swap])
                ) {
                    swap = rightChildIndex;
                }
            }

            if (swap === null) break;

            this.heap[index] = this.heap[swap];
            index = swap;
        }

        this.heap[index] = element;
    }
}
```

