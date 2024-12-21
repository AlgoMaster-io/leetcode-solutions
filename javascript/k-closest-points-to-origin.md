# [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches

1. [Basic: Sorting](#basic-sorting)
2. [Optimized: Max-Heap](#optimized-max-heap)
3. [Most Optimal: Quickselect](#most-optimal-quickselect)

### Basic: Sorting

Intuition:

The problem can be solved by calculating the Euclidean distance from the origin (0, 0) for each point and then simply sorting these points by this distance. After sorting, the first `K` points in the sorted list will be the ones closest to the origin.

Steps:
1. Calculate the distance of each point from the origin.
2. Sort the array of points based on their distance calculated.
3. Return the first `K` points from the sorted array.

```javascript
var kClosest = function(points, K) {
    // Calculate the distance, sort the array and return the first K points
    return points.sort((a, b) => (a[0] ** 2 + a[1] ** 2) - (b[0] ** 2 + b[1] ** 2))
                 .slice(0, K);
};
```

- **Time Complexity**: O(N log N), where N is the number of points. This is due to the sorting step.
- **Space Complexity**: O(1), additional space is minimal as we sort in place.

### Optimized: Max-Heap

Intuition:

Instead of sorting the entire list, we can maintain a max-heap of size `K`. For each point, we calculate its distance and maintain only the `K` smallest distances in the heap. Once all points have been processed, the heap will contain the `K` closest points to the origin.

Steps:
1. Create a max heap of size K to store the closest points.
2. For each point, compute its distance from the origin.
3. If the heap contains less than K points, add the current point.
4. If the heap already has K points, only add the current point if it is closer than the farthest point in the heap.
5. Return the points from the heap.

```javascript
var kClosest = function(points, K) {
    // Helper to calculate distance squared (avoiding sqrt for efficiency)
    const distance = ([x, y]) => x * x + y * y;
    
    // Use a max heap to keep track of K closest points
    const heap = new MaxHeap(K);

    points.forEach(point => {
        const dist = distance(point);
        heap.add([dist, point]);
    });

    return heap.toArray().map(item => item[1]);
};

// MaxHeap implementation using a binary heap
class MaxHeap {
    constructor(capacity) {
        this.data = [];
        this.capacity = capacity;
    }

    add(item) {
        if (this.data.length < this.capacity) {
            this.data.push(item);
            this.bubbleUp(this.data.length - 1);
        } else if (this.data[0][0] > item[0]) {
            this.data[0] = item;
            this.bubbleDown(0);
        }
    }

    bubbleUp(index) {
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.data[parentIndex][0] < this.data[index][0]) {
                [this.data[parentIndex], this.data[index]] = [this.data[index], this.data[parentIndex]];
                index = parentIndex;
            } else {
                break;
            }
        }
    }

    bubbleDown(index) {
        const length = this.data.length;
        while (true) {
            let largest = index;
            const left = 2 * index + 1;
            const right = 2 * index + 2;

            if (left < length && this.data[left][0] > this.data[largest][0]) {
                largest = left;
            }
            if (right < length && this.data[right][0] > this.data[largest][0]) {
                largest = right;
            }
            if (largest !== index) {
                [this.data[index], this.data[largest]] = [this.data[largest], this.data[index]];
                index = largest;
            } else {
                break;
            }
        }
    }

    toArray() {
        return this.data;
    }
}
```

- **Time Complexity**: O(N log K), where N is the number of points and K is the capacity of the heap.
- **Space Complexity**: O(K), for storing the K closest points.

### Most Optimal: Quickselect

Intuition:

Using a quickselect algorithm, similar to quicksort, we can partition the points based on their distance to the origin and recursively find the `K` closest points without sorting the entire list. This approach provides a better average time complexity than O(N log N).

Steps:
1. Use a partition function similar to quicksort to select a pivot point.
2. Reorder the array such that points to the left of the pivot are closer and those to the right are farther.
3. Recursively apply this process on the partition that contains the Kth closest point.

```javascript
var kClosest = function(points, K) {
    const dist = ([x, y]) => x * x + y * y;

    // Lomuto's partitioning
    const partition = (left, right, pivotIndex) => {
        const pivotValue = dist(points[pivotIndex]);
        [points[pivotIndex], points[right]] = [points[right], points[pivotIndex]];
        let storeIndex = left;
        for (let i = left; i < right; i++) {
            if (dist(points[i]) < pivotValue) {
                [points[storeIndex], points[i]] = [points[i], points[storeIndex]];
                storeIndex++;
            }
        }
        [points[right], points[storeIndex]] = [points[storeIndex], points[right]];
        return storeIndex;
    };

    const quickselect = (left, right, K) => {
        if (left === right) return;
        const pivotIndex = left + Math.floor(Math.random() * (right - left + 1));
        const partitionIndex = partition(left, right, pivotIndex);
        if (K < partitionIndex) {
            quickselect(left, partitionIndex - 1, K);
        } else if (K > partitionIndex) {
            quickselect(partitionIndex + 1, right, K);
        }
    };

    quickselect(0, points.length - 1, K);
    return points.slice(0, K);
};
```

- **Time Complexity**: Average case O(N), Worst case O(N^2) 
- **Space Complexity**: O(1), in-place partitioning.

These three solutions provide a gradient of optimization, from basic sorting to an advanced quickselect algorithm, offering various levels of efficiency depending on your needs and constraints.

