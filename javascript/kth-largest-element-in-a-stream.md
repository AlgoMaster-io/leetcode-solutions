# [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches

1. [Use a Sorted Array to Keep Track of Elements](#1-use-a-sorted-array-to-keep-track-of-elements)
2. [Use a Min Heap for Efficient Element Tracking](#2-use-a-min-heap-for-efficient-element-tracking)

### 1. Use a Sorted Array to Keep Track of Elements

**Intuition:**

A simple approach is to maintain a sorted array of all elements we have seen so far. Whenever a new element is added, we insert it into the correct position to maintain the order. This allows us to directly access the k-th largest element, which will be at the position `length - k` in the sorted array.

**Implementation:**

```javascript
class KthLargest {
    constructor(k, nums) {
        this.k = k;
        this.nums = nums.sort((a, b) => a - b);  // Step 1: Sort the initial array
    }
    
    add(val) {
        // Step 2: Insert the new element in a sorted way
        let index = this.binaryInsertIndex(val);
        this.nums.splice(index, 0, val);
        
        // Step 3: Return the k-th largest element
        return this.nums[this.nums.length - this.k];
    }
    
    binaryInsertIndex(val) {
        let low = 0, high = this.nums.length;
        
        // Step 4: Binary search to find the correct insertion position
        while(low < high) {
            let mid = Math.floor(low + (high - low) / 2);
            if (this.nums[mid] < val) low = mid + 1;
            else high = mid;
        }
        return low;
    }
}
```

**Complexity Analysis:**

- Time Complexity:
  - Initialization: O(n log n) due to sorting the initial list.
  - `add`: O(n) due to the array insertion.

- Space Complexity: O(n) for storing the elements.

---

### 2. Use a Min Heap for Efficient Element Tracking

**Intuition:**

Instead of maintaining a sorted list of all elements, we can use a min-heap that only keeps track of the k largest elements. The smallest element in the heap will be the k-th largest overall, and this provides an efficient way to insert and retrieve elements with the guarantee that we're only focusing on the top k largest.

**Implementation:**

```javascript
class KthLargest {
    constructor(k, nums) {
        this.k = k;
        this.minHeap = [];
        
        // Step 1: Add the initial elements to the heap
        nums.forEach(num => this.add(num));
    }

    add(val) {
        // Step 2: Maintain a heap of size k
        if (this.minHeap.length < this.k) {
            this.insertHeap(val);
        } else if (val > this.minHeap[0]) {
            this.removeTop();
            this.insertHeap(val);
        }
        
        // Step 3: Return the smallest element which is the k-th largest
        return this.minHeap[0];
    }

    insertHeap(val) {
        // Step 4: Insert the element at the correct position
        this.minHeap.push(val);
        this.minHeap.sort((a, b) => a - b);
    }
    
    removeTop() {
        // Step 5: Remove the smallest element (root)
        this.minHeap.shift();
    }
}
```

**Complexity Analysis:**

- Time Complexity:
  - Initialization: O(n log k) (each insertion to the heap of size k takes log k time).
  - `add`: O(log k) for insertions.

- Space Complexity: O(k) since the heap only maintains the k largest elements.

The first approach is easy to understand but less efficient for large sequences of calls to `add`. The second approach achieves better performance through efficient memory use and operations, focusing computation only on the largest k elements.

