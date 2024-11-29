# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
```javascript
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap

class KthLargest {
    constructor(k, nums) {
        this.k = k;
        this.minHeap = []; // Min-heap to store the k largest elements
        
        // Add all elements to the heap and maintain only k elements
        for (let num of nums) {
            this.add(num);
        }
    }
    
    add(val) {
        // Add the new value to the heap
        if (this.minHeap.length < this.k) {
            this._insert(val);
        } else if (val > this.minHeap[0]) {
            this.minHeap[0] = val;
            this._heapifyDown();
        }
        
        return this.minHeap[0]; // The kth largest element is the root of the heap
    }
    
    _heapifyUp() {
        let index = this.minHeap.length - 1;
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            if (this.minHeap[parentIndex] <= this.minHeap[index]) break;
            [this.minHeap[parentIndex], this.minHeap[index]] = [this.minHeap[index], this.minHeap[parentIndex]];
            index = parentIndex;
        }
    }
    
    _heapifyDown() {
        let index = 0;
        const length = this.minHeap.length;
        const element = this.minHeap[0];
        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swapIndex = null;
            
            if (leftChildIndex < length) {
                if (this.minHeap[leftChildIndex] < element) {
                    swapIndex = leftChildIndex;
                }
            }
            
            if (rightChildIndex < length) {
                if ((swapIndex === null && this.minHeap[rightChildIndex] < element) || 
                    (swapIndex !== null && this.minHeap[rightChildIndex] < this.minHeap[leftChildIndex])) {
                    swapIndex = rightChildIndex;
                }
            }
            
            if (swapIndex === null) break;
            [this.minHeap[index], this.minHeap[swapIndex]] = [this.minHeap[swapIndex], this.minHeap[index]];
            index = swapIndex;
        }
    }
    
    _insert(val) {
        this.minHeap.push(val);
        this._heapifyUp();
    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * const obj = new KthLargest(k, nums);
 * const param_1 = obj.add(val);
 */
```

