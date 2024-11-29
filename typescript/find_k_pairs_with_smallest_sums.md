# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
typescript
```typescript
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap
function kSmallestPairs(nums1: number[], nums2: number[], k: number): number[][] {
    // Min-heap to store pairs along with their sums
    const minHeap: Array<[number, number, number]> = [];
    const result: number[][] = [];

    // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
    for (let i = 0; i < Math.min(nums1.length, k); i++) {
        minHeap.push([nums1[i], nums2[0], 0]);
    }

    // Comparator for the priority queue (min-heap)
    const comp = (a: [number, number, number], b: [number, number, number]) => {
        return (a[0] + a[1]) - (b[0] + b[1]);
    };

    // Heapify the initial elements
    minHeap.sort(comp);

    // Extract the smallest pairs until we have k pairs or the heap is empty
    while (minHeap.length > 0 && k > 0) {
        const current = minHeap.shift()!;
        result.push([current[0], current[1]]);
        k--;

        // If there are more pairs with the current nums1[i], add them to the heap
        if (current[2] + 1 < nums2.length) {
            minHeap.push([current[0], nums2[current[2] + 1], current[2] + 1]);
            minHeap.sort(comp);
        }
    }

    return result;
}
```

