# [295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approaches
- [Approach 1: Brute Force - Sorting on Demand](#approach-1-brute-force---sorting-on-demand)
- [Approach 2: Two Heaps](#approach-2-two-heaps) (Optimal)

---

### Approach 1: Brute Force - Sorting on Demand

#### Intuition
The brute force approach involves maintaining a list of the numbers we've seen so far and sorting it each time we need to find the median. This is a straightforward approach but very inefficient because as the number of elements increases, sorting each time for a median will be costly.

1. Maintain an array to store incoming numbers.
2. On every insertion, append the new number to the array.
3. To find the median, sort the array.
4. The median is the middle element if the array length is odd, or the average of the two middle elements if the length is even.

#### Code
```javascript
class MedianFinder {
    constructor() {
        this.nums = [];
    }
    
    addNum(num) {
        // Add number to the data stream
        this.nums.push(num);
    }
    
    findMedian() {
        // Sort the array
        const sortedNums = [...this.nums].sort((a, b) => a - b);
        const n = sortedNums.length;
        
        // Return median
        if (n % 2 === 1) {
            return sortedNums[Math.floor(n / 2)]; // Odd length, return the middle element
        } else {
            const mid1 = Math.floor((n-1) / 2);
            const mid2 = Math.floor(n / 2);
            return (sortedNums[mid1] + sortedNums[mid2]) / 2; // Even length, return average of middle elements
        }
    }
}
```

#### Complexity
- **Time Complexity**: 
  - `addNum()`: O(1) for insertion.
  - `findMedian()`: O(n log n) due to sorting.
- **Space Complexity**: O(n) where n is the number of inserted elements.

---

### Approach 2: Two Heaps

#### Intuition
To avoid sorting each time we find a median, we can use two heaps (or priority queues in some languages): a max-heap to keep track of the smaller half of the numbers, and a min-heap to keep track of the larger half.

1. **Max-Heap**: This is used to store the smaller half of the data. The maximum value of this heap is the largest of the smallest elements.
2. **Min-Heap**: This stores the larger half of the data. The minimum value of this heap is the smallest of the largest elements.

The key idea is to maintain these heaps such that:
- The length of max-heap is either equal to or one more than the length of min-heap.
- The elements in max-heap are less than or equal to the elements in min-heap.

To ensure this balance:
- Every time we add a number, we decide which heap to add it to so that the properties mentioned above are maintained.
- To find the median:
  - If both heaps have the same length, the median is the average of the roots of both heaps.
  - If the max-heap has more elements, the root of max-heap is the median.

#### Code
```javascript
class MedianFinder {
    constructor() {
        this.low = new MaxHeap(); // Max-heap
        this.high = new MinHeap(); // Min-heap
    }
    
    addNum(num) {
        // Insert into max heap or min heap
        if (this.low.size() === 0 || num <= this.low.peek()) {
            this.low.add(num);
        } else {
            this.high.add(num);
        }
        
        // Balance heap sizes
        if (this.low.size() > this.high.size() + 1) {
            this.high.add(this.low.poll());
        } else if (this.high.size() > this.low.size()) {
            this.low.add(this.high.poll());
        }
    }
    
    findMedian() {
        if (this.low.size() > this.high.size()) {
            return this.low.peek(); // Max-heap has more elements
        } else {
            return (this.low.peek() + this.high.peek()) / 2; // Both heaps are balanced
        }
    }
}

// Helper classes for the heaps
class MinHeap {
    constructor() {
        this.heap = [];
    }
    
    add(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    
    peek() {
        return this.heap[0];
    }
    
    poll() {
        const min = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = end;
            this.bubbleDown();
        }
        return min;
    }
    
    size() {
        return this.heap.length;
    }
    
    bubbleUp() {
        let index = this.heap.length - 1;
        const element = this.heap[index];
        
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            let parent = this.heap[parentIndex];
            
            if (element >= parent) break;
            this.heap[index] = parent;
            index = parentIndex;
        }
        this.heap[index] = element;
    }
    
    bubbleDown() {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        
        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;
            
            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (leftChild < element) {
                    swap = leftChildIndex;
                }
            }
            
            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if ((swap === null && rightChild < element) || (swap !== null && rightChild < leftChild)) {
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

class MaxHeap {
    constructor() {
        this.heap = [];
    }
    
    add(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    
    peek() {
        return this.heap[0];
    }
    
    poll() {
        const max = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = end;
            this.bubbleDown();
        }
        return max;
    }
    
    size() {
        return this.heap.length;
    }
    
    bubbleUp() {
        let index = this.heap.length - 1;
        const element = this.heap[index];
        
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            let parent = this.heap[parentIndex];
            
            if (element <= parent) break;
            this.heap[index] = parent;
            index = parentIndex;
        }
        this.heap[index] = element;
    }
    
    bubbleDown() {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[0];
        
        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;
            
            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (leftChild > element) {
                    swap = leftChildIndex;
                }
            }
            
            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if ((swap === null && rightChild > element) || (swap !== null && rightChild > leftChild)) {
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

#### Complexity
- **Time Complexity**: 
  - `addNum()`: O(log n) to add an element to one of the heaps.
  - `findMedian()`: O(1) as the median can be fetched directly.
- **Space Complexity**: O(n) where n is the number of inserted elements.

The two heaps approach balances efficiency with simplicity and is typically the preferred method for this problem.

