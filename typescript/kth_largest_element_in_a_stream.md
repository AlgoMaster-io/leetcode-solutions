# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap

class KthLargest {
    private minHeap: number[];
    private k: number;

    constructor(k: number, nums: number[]) {
        this.k = k;
        this.minHeap = [];
        
        // Add all elements to the heap and maintain only k elements
        for (const num of nums) {
            this.add(num);
        }
    }

    add(val: number): number {
        this.insertToHeap(val); // Add the new value to the heap

        // Remove the smallest element if the heap exceeds size k
        if (this.minHeap.length > this.k) {
            this.minHeap.shift(); // Remove the smallest element
        }

        return this.minHeap[0]; // The kth largest element is the root of the heap
    }

    private insertToHeap(num: number) {
        this.minHeap.push(num);
        this.minHeap.sort((a, b) => a - b); // Maintain min-heap property
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * let obj = new KthLargest(k, nums);
 * let param_1 = obj.add(val);
 */
```

