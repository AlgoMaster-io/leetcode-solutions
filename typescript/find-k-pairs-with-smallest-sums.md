# [LeetCode 373: Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

In this problem, we are given two sorted arrays `nums1` and `nums2` and an integer `k`. Our task is to find the k pairs `(u,v)` where `u` is an element from `nums1` and `v` is an element from `nums2` such that their sum `(u + v)` is the smallest among all possible pairs.

## Approaches:
1. [Naive Brute Force](#naive-brute-force)
2. [Optimized with Min-Heap](#optimized-with-min-heap)

## Naive Brute Force

### Intuition
In the naive approach, we generate all possible pairs and calculate their sums. Then, we sort the pairs based on their sums and pick the first `k` pairs. This approach is straightforward but not efficient due to its high time complexity.

### Code
```typescript
function kSmallestPairs(nums1: number[], nums2: number[], k: number): number[][] {
    // Array to store all the possible pairs
    const pairs: [number, number, number][] = [];
    
    // Generate all possible pairs (num1, num2) and store them with their sums
    for (let i = 0; i < nums1.length; i++) {
        for (let j = 0; j < nums2.length; j++) {
            pairs.push([nums1[i], nums2[j], nums1[i] + nums2[j]]);
        }
    }
    
    // Sort the pairs based on their sums
    pairs.sort((a, b) => a[2] - b[2]);
    
    // Return the first k pairs (or all pairs if there are fewer than k)
    return pairs.slice(0, k).map(pair => [pair[0], pair[1]]);
}
```

### Complexity
- **Time Complexity:** O(N * M * log(N * M)), where N is the length of `nums1` and M is the length of `nums2`. This is due to the sorting step.
- **Space Complexity:** O(N * M) to store all possible pairs.

## Optimized with Min-Heap

### Intuition
To optimize the approach, we can use a min-heap to efficiently retrieve the k pairs with the smallest sums. The heap is used to partially order the sums such that we always work with the smallest elements. We initialize the heap with the first pair from each element in `nums1` combined with the first element of `nums2`. As we process each item, we generate the next possible smallest pair by incrementing the index from `nums2`.

### Code
```typescript
function kSmallestPairs(nums1: number[], nums2: number[], k: number): number[][] {
    const result: number[][] = [];
    if (nums1.length === 0 || nums2.length === 0 || k === 0) return result;

    // Min-Heap to store (sum, i, j)
    const heap: [number, number, number][] = [];
    
    // Function to maintain the heap order
    const pushHeap = (i: number, j: number) => {
        if (i < nums1.length && j < nums2.length) {
            heap.push([nums1[i] + nums2[j], i, j]);
            heap.sort((a, b) => a[0] - b[0]);
        }
    };

    // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
    for (let i = 0; i < Math.min(nums1.length, k); i++) {
        pushHeap(i, 0);
    }

    while (result.length < k && heap.length > 0) {
        const [sum, i, j] = heap.shift()!;
        result.push([nums1[i], nums2[j]]);
        // If possible, add the next pair (nums1[i], nums2[j+1])
        pushHeap(i, j + 1);
    }

    return result;
}
```

### Complexity
- **Time Complexity:** O(k * log(min(N, k))), where N is the length of `nums1`. This is because while loops run `k` times and we maintain heap order in `log(min(N, k))` time.
- **Space Complexity:** O(min(N, k)) to store the heap.

This optimized approach provides a balance between intuitive understanding and computational efficiency by leveraging the properties of heaps to minimize the sorting operations.

