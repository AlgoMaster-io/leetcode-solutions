# [Leetcode 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## Table of Contents

- [Approach 1: Naive Sorting](#approach-1-naive-sorting)
- [Approach 2: Min-Heap](#approach-2-min-heap)
- [Approach 3: Binary Search](#approach-3-binary-search)

---

## Approach 1: Naive Sorting

### Intuition
The simplest way to find the \(k^{th}\) smallest element is to flatten the matrix into a single list, sort this list, and then simply pick the \(k^{th}\) element. This works due to the properties of a sorted matrix where each row and column is sorted in ascending order.

### Solution

```typescript
function kthSmallest(matrix: number[][], k: number): number {
    const n = matrix.length;
    const list: number[] = [];

    // Flatten the matrix into a single list
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            list.push(matrix[i][j]);
        }
    }

    // Sort the list
    list.sort((a, b) => a - b);

    // Return the k-1 indexed element since k is 1-based
    return list[k - 1];
}
```

### Complexity
- **Time Complexity**: \(O(n^2 \log n)\), as flattening the matrix takes \(O(n^2)\) and sorting the list takes \(O(n^2 \log n^2)\).
- **Space Complexity**: \(O(n^2)\) for storing the flattened list.

---

## Approach 2: Min-Heap

### Intuition
Since the matrix is sorted, we can use a min-heap (priority queue) to efficiently fetch the smallest elements. We push the first element of each row into the heap and then keep only k smallest elements in the heap by pushing the next smallest item from the respective row where we popped the smallest element.

### Solution

```typescript
function kthSmallest(matrix: number[][], k: number): number {
    const n = matrix.length;
    // Our min-heap (priority queue) will contain triples of [value, row, column]
    const minHeap: [number, number, number][] = [];

    // Initialize the heap with the first element from each row
    for (let i = 0; i < n; i++) {
        minHeap.push([matrix[i][0], i, 0]);
    }

    // This turns the array into a min-heap based on the value
    minHeap.sort((a, b) => a[0] - b[0]);

    // Extract the smallest elements k times
    let number = 0;
    while (k--) {
        const [value, row, col] = minHeap.shift()!;
        number = value;

        // If there is a next column element in the same row, push it to the heap
        if (col + 1 < n) {
            const newItem = [matrix[row][col + 1], row, col + 1];
            minHeap.push(newItem);
            minHeap.sort((a, b) => a[0] - b[0]);
        }
    }

    return number;
}
```

### Complexity
- **Time Complexity**: \(O(k \log n)\), since we perform insertions into a heap of size at most \(n\) for \(k\) elements.
- **Space Complexity**: \(O(n)\) for the min-heap storage.

---

## Approach 3: Binary Search

### Intuition
Given that both rows and columns of the matrix are sorted, a binary search on values is a valid optimization. We set the initial range of the search to the smallest and largest values in the matrix. For each selected middle value, we count how many numbers are less than or equal to it, which can be done in \(O(n)\) time by utilizing the sorted properties of rows. 

### Solution

```typescript
function kthSmallest(matrix: number[][], k: number): number {
    const n = matrix.length;
    let left = matrix[0][0];
    let right = matrix[n - 1][n - 1];

    // Binary search on the value range
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        let count = 0;

        // Count how many numbers are less than or equal to mid
        for (let i = 0; i < n; i++) {
            let j = 0;
            while (j < n && matrix[i][j] <= mid) {
                j++;
            }
            count += j;
        }

        // Adjust the search range based on the count
        if (count < k) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }

    return left;
}
```

### Complexity
- **Time Complexity**: \(O(n \log(max - min))\). We run binary search on the range of numbers \((min, max)\), and for each candidate middle number, we count elements ≤ mid in \(O(n)\) time.
- **Space Complexity**: \(O(1)\), since we're using constant extra space.

