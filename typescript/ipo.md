# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps

class Solution {
    medianSlidingWindow(nums: number[], k: number): number[] {
        const n = nums.length;
        const result: number[] = new Array(n - k + 1);

        // Max-heap for the smaller half
        const maxHeap = new MaxHeap<number>();
        // Min-heap for the larger half
        const minHeap = new MinHeap<number>();
        
        for (let i = 0; i < n; i++) {
            // Add the new element into the appropriate heap
            if (maxHeap.isEmpty() || nums[i] <= maxHeap.peek()) {
                maxHeap.add(nums[i]);
            } else {
                minHeap.add(nums[i]);
            }
            
            // Rebalance the heaps if necessary
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.add(maxHeap.poll());
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.add(minHeap.poll());
            }
            
            // Remove the element that is sliding out of the window
            if (i >= k) {
                const elementToRemove = nums[i - k];
                if (elementToRemove <= maxHeap.peek()) {
                    maxHeap.remove(elementToRemove);
                } else {
                    minHeap.remove(elementToRemove);
                }
                
                // Rebalance the heaps after removal
                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.add(maxHeap.poll());
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.add(minHeap.poll());
                }
            }
            
            // Add the median to the result when the window is fully formed
            if (i >= k - 1) {
                if (maxHeap.size() === minHeap.size()) {
                    result[i - k + 1] = (maxHeap.peek() + minHeap.peek()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.peek();
                }
            }
        }
        
        return result;
    }
}

interface IHeap<T> {
    add(value: T): void;
    peek(): T;
    poll(): T;
    remove(value: T): void;
    size(): number;
    isEmpty(): boolean;
}

class MinHeap<T> implements IHeap<T> {
    private heap: T[] = [];
    
    add(value: T): void {
        this.heap.push(value);
        this.heapifyUp();
    }

    peek(): T {
        return this.heap[0];
    }

    poll(): T {
        const root = this.peek();
        const last = this.heap.pop();
        if (this.heap.length > 0 && last !== undefined) {
            this.heap[0] = last;
            this.heapifyDown();
        }
        return root;
    }

    remove(value: T): void {
        const index = this.heap.indexOf(value);
        if (index !== -1) {
            const last = this.heap.pop();
            if (index !== this.heap.length && last !== undefined) {
                this.heap[index] = last;
                this.heapifyUp(index);
                this.heapifyDown(index);
            }
        }
    }

    size(): number {
        return this.heap.length;
    }

    isEmpty(): boolean {
        return this.size() === 0;
    }

    private heapifyUp(index?: number): void {
        let currentIndex = index ?? this.size() - 1;
        while (currentIndex > 0) {
            const parentIndex = Math.floor((currentIndex - 1) / 2);
            if (this.heap[currentIndex] >= this.heap[parentIndex]) break;
            [this.heap[currentIndex], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[currentIndex]];
            currentIndex = parentIndex;
        }
    }

    private heapifyDown(index: number = 0): void {
        let currentIndex = index;
        while (currentIndex < this.size()) {
            const leftIndex = 2 * currentIndex + 1;
            const rightIndex = 2 * currentIndex + 2;
            let smallest = currentIndex;

            if (leftIndex < this.size() && this.heap[leftIndex] < this.heap[smallest]) {
                smallest = leftIndex;
            }
            if (rightIndex < this.size() && this.heap[rightIndex] < this.heap[smallest]) {
                smallest = rightIndex;
            }

            if (smallest === currentIndex) break;

            [this.heap[currentIndex], this.heap[smallest]] = [this.heap[smallest], this.heap[currentIndex]];
            currentIndex = smallest;
        }
    }
}

class MaxHeap<T> implements IHeap<T> {
    private heap: T[] = [];

    add(value: T): void {
        this.heap.push(value);
        this.heapifyUp();
    }

    peek(): T {
        return this.heap[0];
    }

    poll(): T {
        const root = this.peek();
        const last = this.heap.pop();
        if (this.heap.length > 0 && last !== undefined) {
            this.heap[0] = last;
            this.heapifyDown();
        }
        return root;
    }

    remove(value: T): void {
        const index = this.heap.indexOf(value);
        if (index !== -1) {
            const last = this.heap.pop();
            if (index !== this.heap.length && last !== undefined) {
                this.heap[index] = last;
                this.heapifyUp(index);
                this.heapifyDown(index);
            }
        }
    }

    size(): number {
        return this.heap.length;
    }

    isEmpty(): boolean {
        return this.size() === 0;
    }

    private heapifyUp(index?: number): void {
        let currentIndex = index ?? this.size() - 1;
        while (currentIndex > 0) {
            const parentIndex = Math.floor((currentIndex - 1) / 2);
            if (this.heap[currentIndex] <= this.heap[parentIndex]) break;
            [this.heap[currentIndex], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[currentIndex]];
            currentIndex = parentIndex;
        }
    }

    private heapifyDown(index: number = 0): void {
        let currentIndex = index;
        while (currentIndex < this.size()) {
            const leftIndex = 2 * currentIndex + 1;
            const rightIndex = 2 * currentIndex + 2;
            let largest = currentIndex;

            if (leftIndex < this.size() && this.heap[leftIndex] > this.heap[largest]) {
                largest = leftIndex;
            }
            if (rightIndex < this.size() && this.heap[rightIndex] > this.heap[largest]) {
                largest = rightIndex;
            }

            if (largest === currentIndex) break;

            [this.heap[currentIndex], this.heap[largest]] = [this.heap[largest], this.heap[currentIndex]];
            currentIndex = largest;
        }
    }
}

```

