# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
typescript
```typescript
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap

function kClosest(points: number[][], k: number): number[][] {
    // Max-Heap to store the k closest points using a custom comparator
    const maxHeap: {point: number[], distance: number}[] = [];

    const distance = (point: number[]): number => 
        point[0] * point[0] + point[1] * point[1];

    for (const point of points) {
        const dist = distance(point);
        if (maxHeap.length < k) {
            maxHeap.push({point, distance: dist});
            maxHeap.sort((a, b) => b.distance - a.distance);
        } else if (dist < maxHeap[0].distance) {
            maxHeap[0] = {point, distance: dist};
            maxHeap.sort((a, b) => b.distance - a.distance);
        }
    }

    return maxHeap.map(entry => entry.point);
}
```

## Approach 2: Quickselect

### Solution
typescript
```typescript
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place

function kClosest(points: number[][], k: number): number[][] {
    const quickSelect = (left: number, right: number, k: number): void => {
        if (left >= right) return;

        const pivotIndex = partition(left, right);
        if (pivotIndex === k) {
            return;
        } else if (pivotIndex < k) {
            quickSelect(pivotIndex + 1, right, k);
        } else {
            quickSelect(left, pivotIndex - 1, k);
        }
    };

    const partition = (left: number, right: number): number => {
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

    const swap = (points: number[][], i: number, j: number): void => {
        const temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    };

    const distance = (point: number[]): number => 
        point[0] * point[0] + point[1] * point[1];

    quickSelect(0, points.length - 1, k);
    return points.slice(0, k);
}
```

