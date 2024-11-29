# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(k)
function maxSlidingWindow(nums, k) {
    const n = nums.length;
    const result = new Array(n - k + 1);
    const deque = []; // Stores indices of elements

    for (let i = 0; i < n; i++) {
        // Remove indices out of the current window
        if (deque.length > 0 && deque[0] < i - k + 1) {
            deque.shift();
        }

        // Remove elements smaller than the current element from the back
        while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
            deque.pop();
        }

        deque.push(i); // Add the current element's index to the deque

        // Add the maximum element of the current window to the result
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque[0]];
        }
    }

    return result;
}
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
```javascript
// Time Complexity: O(n log k)
// Space Complexity: O(k)
class MaxHeap {
    constructor() {
        this.heap = [];
    }

    offer(e) {
        this.heap.push(e);
        this._bubbleUp(this.heap.length - 1);
    }

    poll() {
        if (this.heap.length === 1) return this.heap.pop();
        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this._trickleDown(0);
        return max;
    }

    peek() {
        return this.heap[0];
    }

    _bubbleUp(index) {
        const parentIndex = Math.floor((index - 1) / 2);
        if (index <= 0 || this._compare(parentIndex, index)) return;
        this._swap(index, parentIndex);
        this._bubbleUp(parentIndex);
    }

    _trickleDown(index) {
        let largest = index;
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        if (left < this.heap.length && !this._compare(largest, left)) largest = left;
        if (right < this.heap.length && !this._compare(largest, right)) largest = right;
        if (largest !== index) {
            this._swap(index, largest);
            this._trickleDown(largest);
        }
    }

    _compare(i, j) {
        return this.heap[i][0] > this.heap[j][0];
    }

    _swap(i, j) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

function maxSlidingWindow(nums, k) {
    const n = nums.length;
    const result = new Array(n - k + 1);
    const maxHeap = new MaxHeap(); // Max-heap: {value, index}

    for (let i = 0; i < n; i++) {
        // Add the current element to the heap
        maxHeap.offer([nums[i], i]);

        // Remove elements that are out of the current window
        while (maxHeap.peek()[1] < i - k + 1) {
            maxHeap.poll();
        }

        // Add the maximum element of the current window to the result
        if (i >= k - 1) {
            result[i - k + 1] = maxHeap.peek()[0];
        }
    }

    return result;
}
```

