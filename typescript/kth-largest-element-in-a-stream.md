# [Leetcode 703: Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches
1. [Basic Approach: Sorted Array](#basic-approach-sorted-array)
2. [Optimal Approach: Min-Heap/Priority Queue](#optimal-approach-min-heap-priority-queue)

---

### Basic Approach: Sorted Array

**Intuition**: 
The simplest way to find the kth largest element is to maintain a sorted list of elements added so far. Whenever a new element is added to the stream, we insert it at the correct position in the sorted list. The kth largest element will then be at the `n-k` index of this sorted array (where `n` is the length of the array).

While this is straightforward, the main drawback is the insertion time complexity, which can be expensive for large input streams.

**Code**:
```typescript
class KthLargest {
    private k: number;
    private stream: number[];

    constructor(k: number, nums: number[]) {
        this.k = k;
        this.stream = nums.sort((a, b) => a - b); // initially sort the array
    }

    add(val: number): number {
        // Find the correct position to insert the new value to keep the array sorted
        let left = 0, right = this.stream.length;
        while (left < right) {
            const mid = left + Math.floor((right - left) / 2);
            if (this.stream[mid] < val) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        // Insert val at the correct position
        this.stream.splice(left, 0, val);

        // Return the kth largest element
        return this.stream[this.stream.length - this.k];
    }
}
```

**Time Complexity**: 
- `add(val)`: O(n) due to the array insertion operation.
  
**Space Complexity**: 
- O(n), where `n` is the number of elements in the stream.

---

### Optimal Approach: Min-Heap/Priority Queue

**Intuition**:
A more efficient way to keep track of the kth largest element is using a min-heap of size `k`. The idea is to maintain a heap of the largest `k` elements seen so far. The smallest element in this heap is the kth largest overall.

This approach allows us to efficiently add new elements and maintain the kth largest element, as heap operations (insertion and deletion) are logarithmic in time complexity relative to the number of elements in the heap.

**Code**:
```typescript
class MinHeap {
    private heap: number[];

    constructor() {
        this.heap = [];
    }

    push(val: number): void {
        this.heap.push(val);
        this.bubbleUp();
    }

    pop(): number {
        const minValue = this.heap[0];
        const lastValue = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = lastValue!;
            this.bubbleDown();
        }
        return minValue;
    }

    peek(): number {
        return this.heap[0];
    }

    size(): number {
        return this.heap.length;
    }

    private bubbleUp(): void {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[parentIndex] <= this.heap[index]) break;
            [this.heap[parentIndex], this.heap[index]] = [this.heap[index], this.heap[parentIndex]];
            index = parentIndex;
        }
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
                if (this.heap[leftChildIndex] < element) {
                    swap = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                if (
                    (swap === null && this.heap[rightChildIndex] < element) ||
                    (swap !== null && this.heap[rightChildIndex] < this.heap[leftChildIndex])
                ) {
                    swap = rightChildIndex;
                }
            }

            if (swap === null) break;
            [this.heap[index], this.heap[swap]] = [this.heap[swap], this.heap[index]];
            index = swap;
        }
    }
}

class KthLargest {
    private heap: MinHeap;
    private k: number;

    constructor(k: number, nums: number[]) {
        this.k = k;
        this.heap = new MinHeap();

        // Insert initial numbers into the heap
        for (let num of nums) {
            this.add(num);
        }
    }

    add(val: number): number {
        // Add new value to the heap
        this.heap.push(val);

        // Keep the heap size to k by removing smallest (top) element
        if (this.heap.size() > this.k) {
            this.heap.pop();
        }

        // The root of the heap is the kth largest element
        return this.heap.peek();
    }
}
```

**Time Complexity**:
- `add(val)`: O(log k) for insertion into the heap.

**Space Complexity**:
- O(k), as we are maintaining a heap of size `k`.

