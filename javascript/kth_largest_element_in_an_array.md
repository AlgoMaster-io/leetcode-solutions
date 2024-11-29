# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
javascript
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(1)
function findKthLargest(nums, k) {
    nums.sort((a, b) => a - b); // Sort the array in ascending order
    return nums[nums.length - k]; // Return the kth largest element
}
```

## Approach 2: Using a Min-Heap

### Solution
javascript
```javascript
// Time Complexity: O(n log k)
// Space Complexity: O(k)
function findKthLargest(nums, k) {
    const minHeap = new MinHeap(); // Custom Min-Heap class instance

    for (let num of nums) {
        minHeap.add(num); // Add the current number to the heap
        if (minHeap.size() > k) {
            minHeap.poll(); // Remove the smallest element if the heap size exceeds k
        }
    }

    return minHeap.peek(); // The root of the heap is the kth largest element
}

// Helper Min-Heap implementation
class MinHeap {
    constructor() {
        this.heap = [];
    }

    add(value) {
        this.heap.push(value);
        this.bubbleUp();
    }

    bubbleUp() {
        let index = this.heap.length - 1;
        while (index > 0) {
            const element = this.heap[index];
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];

            if (parent <= element) break;
            this.heap[index] = parent;
            this.heap[parentIndex] = element;
            index = parentIndex;
        }
    }

    poll() {
        const min = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = end;
            this.sinkDown();
        }
        return min;
    }

    sinkDown() {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[index];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (leftChild < element) swap = leftChildIndex;
            }

            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if ((swap === null && rightChild < element) || (swap !== null && rightChild < leftChild)) {
                    swap = rightChildIndex;
                }
            }

            if (swap === null) break;
            this.heap[index] = this.heap[swap];
            this.heap[swap] = element;
            index = swap;
        }
    }

    peek() {
        return this.heap[0];
    }

    size() {
        return this.heap.length;
    }
}
```

## Approach 3: Quickselect (Partitioning)

### Solution
javascript
```javascript
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
function findKthLargest(nums, k) {
    const n = nums.length;
    return quickSelect(nums, 0, n - 1, n - k);
}

function quickSelect(nums, left, right, k) {
    if (left === right) {
        return nums[left]; // Base case: only one element
    }

    const randomIndex = Math.floor(Math.random() * (right - left + 1)) + left;
    const pivotIndex = partition(nums, left, right, randomIndex);

    if (pivotIndex === k) {
        return nums[k]; // Found the kth largest element
    } else if (pivotIndex < k) {
        return quickSelect(nums, pivotIndex + 1, right, k); // Search in the right part
    } else {
        return quickSelect(nums, left, pivotIndex - 1, k); // Search in the left part
    }
}

function partition(nums, left, right, pivotIndex) {
    const pivotValue = nums[pivotIndex];
    swap(nums, pivotIndex, right); // Move pivot to the end
    let storeIndex = left;

    for (let i = left; i < right; i++) {
        if (nums[i] < pivotValue) {
            swap(nums, storeIndex, i);
            storeIndex++;
        }
    }

    swap(nums, storeIndex, right); // Move pivot to its final position
    return storeIndex;
}

function swap(nums, i, j) {
    const temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

