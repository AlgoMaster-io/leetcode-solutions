# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
typescript
```typescript
// Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
// Space Complexity: O(k), for the max-heap

function mincostToHireWorkers(quality: number[], wage: number[], k: number): number {
    const n = quality.length;
    const workers: number[][] = new Array(n);

    // Create an array of workers with their wage-to-quality ratio and quality
    for (let i = 0; i < n; i++) {
        workers[i] = [(wage[i] / quality[i]), quality[i]];
    }

    // Sort workers by their wage-to-quality ratio
    workers.sort((a, b) => a[0] - b[0]);

    const maxHeap: number[] = [];
    let totalQuality = 0; // Sum of qualities in the current group
    let minCost = Number.MAX_VALUE;

    for (let worker of workers) {
        const currentQuality = worker[1];
        totalQuality += currentQuality;
        maxHeap.push(currentQuality);
        maxHeap.sort((a, b) => b - a); // Sort maxHeap in descending order

        // If we exceed k workers, remove the one with the highest quality
        if (maxHeap.length > k) {
            totalQuality -= maxHeap.shift()!;
        }

        // If we have exactly k workers, calculate the cost
        if (maxHeap.length === k) {
            minCost = Math.min(minCost, totalQuality * worker[0]);
        }
    }

    return minCost;
}
```

