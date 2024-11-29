# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution

typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)

class Solution {
    public findMaxValueOfEquation(points: number[][], k: number): number {
        const maxHeap = new MaxHeap<number[]>((a, b) => b[0] - a[0]); // Max-heap: {y - x, x}
        let maxValue = Number.MIN_SAFE_INTEGER;

        for (const [x, y] of points) {
            // Remove points outside the range of k
            while (!maxHeap.isEmpty() && x - maxHeap.peek()[1] > k) {
                maxHeap.poll();
            }

            // If the heap is not empty, calculate the current value of the equation
            if (!maxHeap.isEmpty()) {
                maxValue = Math.max(maxValue, maxHeap.peek()[0] + y + x);
            }

            // Add the current point to the heap
            maxHeap.offer([y - x, x]);
        }

        return maxValue;
    }
}

// Helper class for Max Heap (Priority Queue)
class MaxHeap<T> {
    private _data: T[];
    private _compare: (a: T, b: T) => number;

    constructor(compare: (a: T, b: T) => number) {
        this._data = [];
        this._compare = compare;
    }

    isEmpty(): boolean {
        return this._data.length === 0;
    }

    peek(): T {
        return this._data[0];
    }

    offer(value: T): void {
        this._data.push(value);
        this._siftUp();
    }

    poll(): void {
        if (this._data.length === 1) {
            this._data.pop();
        } else {
            [this._data[0], this._data[this._data.length - 1]] = [this._data[this._data.length - 1], this._data[0]];
            this._data.pop();
            this._siftDown();
        }
    }

    private _siftUp(): void {
        let nodeIdx = this._data.length - 1;

        while (nodeIdx > 0) {
            const parentIdx = Math.floor((nodeIdx - 1) / 2);

            if (this._compare(this._data[parentIdx], this._data[nodeIdx]) >= 0) {
                break;
            }

            [this._data[parentIdx], this._data[nodeIdx]] = [this._data[nodeIdx], this._data[parentIdx]];
            nodeIdx = parentIdx;
        }
    }

    private _siftDown(): void {
        let nodeIdx = 0;

        while (nodeIdx < this._data.length) {
            const leftChildIdx = 2 * nodeIdx + 1;
            const rightChildIdx = 2 * nodeIdx + 2;
            let largestIdx = nodeIdx;

            if (leftChildIdx < this._data.length && this._compare(this._data[leftChildIdx], this._data[largestIdx]) > 0) {
                largestIdx = leftChildIdx;
            }

            if (rightChildIdx < this._data.length && this._compare(this._data[rightChildIdx], this._data[largestIdx]) > 0) {
                largestIdx = rightChildIdx;
            }

            if (largestIdx === nodeIdx) {
                break;
            }

            [this._data[nodeIdx], this._data[largestIdx]] = [this._data[largestIdx], this._data[nodeIdx]];
            nodeIdx = largestIdx;
        }
    }
}
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution

typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

class Solution {
    public findMaxValueOfEquation(points: number[][], k: number): number {
        const deque: [number, number][] = []; // Stores pairs {y - x, x}
        let maxValue = Number.MIN_SAFE_INTEGER;

        for (const [x, y] of points) {
            // Remove points outside the range of k
            while (deque.length > 0 && x - deque[0][1] > k) {
                deque.shift();
            }

            // If the deque is not empty, calculate the current value of the equation
            if (deque.length > 0) {
                maxValue = Math.max(maxValue, deque[0][0] + y + x);
            }

            // Remove points from the back of the deque that have a smaller y - x value
            while (deque.length > 0 && deque[deque.length - 1][0] <= y - x) {
                deque.pop();
            }

            // Add the current point to the deque
            deque.push([y - x, x]);
        }

        return maxValue;
    }
}
```

