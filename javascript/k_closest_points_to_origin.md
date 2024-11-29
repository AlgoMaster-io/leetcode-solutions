# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
```javascript
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap

const kClosest = (points, k) => {
    // Max-Heap to store the k closest points
    const maxHeap = new MaxHeap((a, b) => 
        distance(b) - distance(a)
    );

    // Add points to the heap and maintain only the k closest points
    for (const point of points) {
        maxHeap.add(point);
        if (maxHeap.size() > k) {
            maxHeap.poll(); // Remove the farthest point
        }
    }

    // Extract the k closest points from the heap
    const result = new Array(k);
    for (let i = 0; i < k; i++) {
        result[i] = maxHeap.poll();
    }

    return result;
};

// Helper method to calculate the squared distance from the origin
const distance = (point) => {
    return point[0] * point[0] + point[1] * point[1];
};

// This is a utility class for Max-Heap
class MaxHeap {
    constructor(compare) {
        this.heap = [];
        this.compare = compare;
    }

    add(val) {
        this.heap.push(val);
        this.bubbleUp(this.heap.length - 1);
    }

    poll() {
        if (this.size() < 2) return this.heap.pop();
        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.bubbleDown(0);
        return max;
    }

    size() {
        return this.heap.length;
    }

    bubbleUp(index) {
        const element = this.heap[index];
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];
            if (this.compare(element, parent) <= 0) break;
            this.heap[index] = parent;
            index = parentIndex;
        }
        this.heap[index] = element;
    }

    bubbleDown(index) {
        const length = this.heap.length;
        const element = this.heap[index];
        
        while (true) {
            let leftIndex = 2 * index + 1;
            let rightIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftIndex < length) {
                leftChild = this.heap[leftIndex];
                if (this.compare(leftChild, element) > 0) {
                    swap = leftIndex;
                }
            }

            if (rightIndex < length) {
                rightChild = this.heap[rightIndex];
                if ((swap === null && this.compare(rightChild, element) > 0) || 
                    (swap !== null && this.compare(rightChild, leftChild) > 0)) {
                    swap = rightIndex;
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

## Approach 2: Quickselect

### Solution
```javascript
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place

const kClosest = (points, k) => {
    quickSelect(points, 0, points.length - 1, k);
    return points.slice(0, k);
};

const quickSelect = (points, left, right, k) => {
    if (left >= right) return;

    const pivotIndex = partition(points, left, right);
    if (pivotIndex === k) {
        return;
    } else if (pivotIndex < k) {
        quickSelect(points, pivotIndex + 1, right, k);
    } else {
        quickSelect(points, left, pivotIndex - 1, k);
    }
};

const partition = (points, left, right) => {
    const pivot = points[right];
    const pivotDistance = distance(pivot);
    let i = left;

    for (let j = left; j < right; j++) {
        if (distance(points[j]) < pivotDistance) {
            swap(points, i, j);
            i++;
        }
    }
    swap(points, i, right);
    return i;
};

const swap = (points, i, j) => {
    const temp = points[i];
    points[i] = points[j];
    points[j] = temp;
};

const distance = (point) => {
    return point[0] * point[0] + point[1] * point[1];
};
```

