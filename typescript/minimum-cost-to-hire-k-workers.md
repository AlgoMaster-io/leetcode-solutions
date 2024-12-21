# [Leetcode 857: Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches:
1. [Greedy with Sorting and Priority Queue](#approach-1)
   
## Approach 1: Greedy with Sorting and Priority Queue

### Intuition:
For each worker, the cost has to be calculated based on the ratio of wage to quality. A worker with a higher ratio means they require higher pay for each unit of effort they provide. To hire the minimum cost of k workers, examine the concept of keeping integrated quality low while maintaining the correct ratio.

The steps are as follows:
1. Calculate the ratio of wage to quality for each worker.
2. Sort the workers based on this ratio in increasing order.
3. Use a min-heap (priority queue) to maintain the k workers with the least quality.
4. For each worker in the sorted list, add the quality to a running total, and maintain this total for the cheapest k workers using the heap.
5. For each iteration once we have at least k workers, calculate the total cost using the running quality sum multiplied by the current worker's ratio. Track the minimum cost.

### Code:
```typescript
function mincostToHireWorkers(quality: number[], wage: number[], K: number): number {
    // Construct workers array with each having its quality, wage, and wage/quality ratio
    const workers = [];
    for (let i = 0; i < quality.length; i++) {
        workers.push({ quality: quality[i], wage: wage[i], ratio: wage[i] / quality[i] });
    }

    // Sort workers by their wage-to-quality ratio
    workers.sort((a, b) => a.ratio - b.ratio);

    // Priority queue (max heap) to maintain the cheapest K quality values
    const maxHeap = new MaxPriorityQueue({ priority: (x) => x });
    let sumQuality = 0;
    let minCost = Infinity;

    // Iterate through sorted workers
    for (const worker of workers) {
        // Add worker quality to our running total and heap
        sumQuality += worker.quality;
        maxHeap.enqueue(worker.quality);

        // If heap size exceeds K, remove the respective maximum quality worker
        if (maxHeap.size() > K) {
            sumQuality -= maxHeap.dequeue().element;
        }

        // If heap has exactly K elements, calculate a new potential price
        if (maxHeap.size() === K) {
            minCost = Math.min(minCost, sumQuality * worker.ratio);
        }
    }

    return minCost;
}
```

### Explanation:
- **Sorting and Priority Queue**: We sort the workers based on their wage-to-quality ratio, ensuring that we prioritize hiring workers with better ratios.
- **Max Heap for Quick Adjustment**: By using a max heap, we ensure that if our running total of quality exceeds K workers, we can efficiently remove the maximum quality worker to maintain or minimize our cost.
- **Cost Calculation**: For each combination of workers with K size, calculate the total cost using the current ratio and running quality total.

### Time Complexity:
- Sorting workers takes \(O(N \log N)\).
- Iterating through workers takes \(O(N \log K)\) due to heap operations. Thus, the overall time complexity is \(O(N \log N + N \log K)\).

### Space Complexity:
- Uses additional space for storing the heap, i.e., \(O(K)\).

This approach efficiently finds the minimum cost using a combination of sorting to manage ratios and a priority queue to manage quality values efficiently.

