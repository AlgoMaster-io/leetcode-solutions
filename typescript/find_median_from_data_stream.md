# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
typescript
```typescript
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream

class MedianFinder {
    private maxHeap: MaxHeap<number>; // To store the smaller half of the numbers
    private minHeap: MinHeap<number>; // To store the larger half of the numbers

    constructor() {
        this.maxHeap = new MaxHeap(); // Max-heap
        this.minHeap = new MinHeap(); // Min-heap
    }

    public addNum(num: number): void {
        // Add to maxHeap, then balance by adding the largest of maxHeap to minHeap
        this.maxHeap.add(num);
        this.minHeap.add(this.maxHeap.poll()!);

        // Ensure maxHeap always has the same number or one more element than minHeap
        if (this.maxHeap.size() < this.minHeap.size()) {
            this.maxHeap.add(this.minHeap.poll()!);
        }
    }

    public findMedian(): number {
        if (this.maxHeap.size() > this.minHeap.size()) {
            return this.maxHeap.peek(); // Odd number of elements
        }
        return (this.maxHeap.peek() + this.minHeap.peek()) / 2.0; // Even number of elements
    }
}

class MinHeap<T> {
    private heap: T[];

    constructor() {
        this.heap = [];
    }

    public add(value: T): void {
        this.heap.push(value);
        this.bubbleUp();
    }

    public poll(): T | undefined {
        if (this.size() > 1) {
            this.swap(0, this.heap.length - 1);
            const poppedValue = this.heap.pop();
            this.bubbleDown();
            return poppedValue;
        }
        return this.heap.pop();
    }

    public peek(): T {
        if (this.size() === 0) {
            throw new Error("Heap is empty");
        }
        return this.heap[0];
    }

    public size(): number {
        return this.heap.length;
    }

    private bubbleUp(): void {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] >= this.heap[parentIndex]) {
                break;
            }
            this.swap(index, parentIndex);
            index = parentIndex;
        }
    }

    private bubbleDown(): void {
        let index = 0;
        const length = this.heap.length;
        while (true) {
            let leftIndex = 2 * index + 1;
            let rightIndex = 2 * index + 2;
            let smallest = index;

            if (leftIndex < length && this.heap[leftIndex] < this.heap[smallest]) {
                smallest = leftIndex;
            }

            if (rightIndex < length && this.heap[rightIndex] < this.heap[smallest]) {
                smallest = rightIndex;
            }

            if (smallest === index) {
                break;
            }

            this.swap(index, smallest);
            index = smallest;
        }
    }

    private swap(i: number, j: number): void {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

class MaxHeap<T> extends MinHeap<T> {
    constructor() {
        super();
    }

    protected bubbleUp(): void {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] <= this.heap[parentIndex]) {
                break;
            }
            this.swap(index, parentIndex);
            index = parentIndex;
        }
    }

    protected bubbleDown(): void {
        let index = 0;
        const length = this.heap.length;
        while (true) {
            let leftIndex = 2 * index + 1;
            let rightIndex = 2 * index + 2;
            let largest = index;

            if (leftIndex < length && this.heap[leftIndex] > this.heap[largest]) {
                largest = leftIndex;
            }

            if (rightIndex < length && this.heap[rightIndex] > this.heap[largest]) {
                largest = rightIndex;
            }

            if (largest === index) {
                break;
            }

            this.swap(index, largest);
            index = largest;
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * const obj = new MedianFinder();
 * obj.addNum(num);
 * const param_2 = obj.findMedian();
 */
```

