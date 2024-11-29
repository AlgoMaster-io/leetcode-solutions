# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
```javascript
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream

class MedianFinder {
    constructor() {
        this.maxHeap = new MaxPriorityQueue(); // To store the smaller half of the numbers
        this.minHeap = new MinPriorityQueue(); // To store the larger half of the numbers
    }

    addNum(num) {
        // Add to maxHeap, then balance by adding the largest of maxHeap to minHeap
        this.maxHeap.enqueue(num);
        this.minHeap.enqueue(this.maxHeap.dequeue().element);

        // Ensure maxHeap always has the same number or one more element than minHeap
        if (this.maxHeap.size() < this.minHeap.size()) {
            this.maxHeap.enqueue(this.minHeap.dequeue().element);
        }
    }

    findMedian() {
        if (this.maxHeap.size() > this.minHeap.size()) {
            return this.maxHeap.front().element; // Odd number of elements
        }
        return (this.maxHeap.front().element + this.minHeap.front().element) / 2.0; // Even number of elements
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * let obj = new MedianFinder();
 * obj.addNum(num);
 * let param_2 = obj.findMedian();
 */
```

**Note:** The JavaScript solution uses `MaxPriorityQueue` and `MinPriorityQueue` from the `@datastructures-js/priority-queue` package because JavaScript does not have a built-in priority queue. Make sure to install the package using npm if required:

```bash
npm install @datastructures-js/priority-queue
```

Add the following imports at the beginning of your file if you need them:

```javascript
const { MaxPriorityQueue } = require('@datastructures-js/priority-queue');
const { MinPriorityQueue } = require('@datastructures-js/priority-queue');
```

