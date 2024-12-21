[Leetcode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Max-Heap](#approach-2-max-heap)
- [Approach 3: Quickselect](#approach-3-quickselect)

### Approach 1: Sorting

#### Intuition
The simplest method is to calculate the Euclidean distance of each point from the origin (0, 0) and sort the points based on these distances. Once sorted, the first `K` points in this sorted list will be the `K` closest points to the origin.

#### Code
```typescript
function kClosest(points: number[][], K: number): number[][] {
    // Calculate the distance for all points and store them with their respective distances.
    return points
        .sort((a, b) => {
            // Calculate squared distances to avoid computing square roots
            const distA = a[0] ** 2 + a[1] ** 2;
            const distB = b[0] ** 2 + b[1] ** 2;
            return distA - distB;
        })
        .slice(0, K); // Return the first K elements from the sorted list
}
```
#### Time Complexity
- O(N log N), where N is the number of points, due to the sorting.

#### Space Complexity
- O(N), required for sorting.

---

### Approach 2: Max-Heap

#### Intuition
Instead of sorting all points which takes O(N log N), we can maintain a max-heap of size `K` that contains the closest points seen so far. By using a max-heap, we ensure that the largest distance is always on top, so whenever we encounter a new point closer than the farthest point in the heap, we replace it.

#### Code
```typescript
function kClosest(points: number[][], K: number): number[][] {
    // Min-Heap with a custom comparator that sorts according to the negative of the distance
    const maxHeap: [number, number[]][] = [];
    
    // Helper function to calculate squared distance
    const squaredDistance = (point: number[]): number => {
        return point[0] ** 2 + point[1] ** 2;
    };

    for (const point of points) {
        const dist = squaredDistance(point);
        if (maxHeap.length < K) {
            maxHeap.push([dist, point]);
            maxHeap.sort((a, b) => b[0] - a[0]); // Sort maxHeap by distance descending
        } else if (dist < maxHeap[0][0]) {
            maxHeap[0] = [dist, point];
            maxHeap.sort((a, b) => b[0] - a[0]); // Re-sort after insertion
        }
    }
    
    return maxHeap.map(e => e[1]);
}
```
#### Time Complexity
- O(N log K) due to maintaining a heap of size `K`.

#### Space Complexity
- O(K) for storing the K closest points in the heap.

---

### Approach 3: Quickselect

#### Intuition
Quickselect is an optimization over QuickSort which allows us to select the `Kth` smallest element in expected O(N) time. By adapting Quickselect, we find that all points closer than the Kth closest can be left in any order, just ensuring the K closest.

#### Code
```typescript
function kClosest(points: number[][], K: number): number[][] {
    const distance = (point: number[]): number => point[0] ** 2 + point[1] ** 2;

    function partition(left: number, right: number, pivotIndex: number): number {
        const pivotDistance = distance(points[pivotIndex]);
        [points[pivotIndex], points[right]] = [points[right], points[pivotIndex]]; // Move pivot to end
        let storeIndex = left;

        for (let i = left; i < right; i++) {
            if (distance(points[i]) < pivotDistance) {
                [points[storeIndex], points[i]] = [points[i], points[storeIndex]];
                storeIndex++;
            }
        }

        [points[right], points[storeIndex]] = [points[storeIndex], points[right]]; // Move pivot to its final place
        return storeIndex;
    }

    function quickselect(left: number, right: number, K: number): void {
        if (left < right) {
            const pivotIndex = Math.floor(left + (right - left) / 2);
            const partitionIndex = partition(left, right, pivotIndex);

            if (K < partitionIndex) {
                quickselect(left, partitionIndex - 1, K);
            } else if (K > partitionIndex) {
                quickselect(partitionIndex + 1, right, K);
            }
        }
    }

    quickselect(0, points.length - 1, K);
    return points.slice(0, K);
}
```
#### Time Complexity
- Expected O(N), worst-case O(N^2) in rare scenarios of bad partitioning.

#### Space Complexity
- O(1), as it rearranges elements in place.

This approach is highly efficient for larger datasets due to its average linear time complexity.

