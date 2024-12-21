[Leetcode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

### Approaches:
- [Approach 1: Naive Brute Force](#approach-1-naive-brute-force)
- [Approach 2: Sorting and Min-Heap](#approach-2-sorting-and-min-heap)

---

## Approach 1: Naive Brute Force

### Intuition:
The naive solution involves checking every interval for each query to find the smallest interval that encompasses the query. This basic approach is straightforward but inefficient for large inputs.

### Detailed Explanation:
1. **Iterate through each query**: For each query, we check all the intervals.
2. **Find eligible intervals**: For the current query, determine which intervals include the query.
3. **Select the smallest interval**: Among the eligible intervals, select the interval with the smallest length.
4. **Store result**: Store the size of this interval for the query.

### Time Complexity:
- **O(m * n)**, where *m* is the number of queries and *n* is the number of intervals. This is because we check each interval for every query.

### Space Complexity:
- **O(1)** (excluding output).

```typescript
function minIntervalBruteForce(intervals: number[][], queries: number[]): number[] {
    const results: number[] = [];
    
    // Iterate through each query
    for (let query of queries) {
        let minSize = Infinity;
        
        // Check each interval
        for (let [start, end] of intervals) {
            // Check if the current interval covers the query
            if (start <= query && query <= end) {
                const size = end - start + 1;
                
                // Update the minimum size if found
                if (size < minSize) {
                    minSize = size;
                }
            }
        }
        
        // Save the result for this query
        results.push(minSize === Infinity ? -1 : minSize);
    }
    
    return results;
}
```

---

## Approach 2: Sorting and Min-Heap

### Intuition:
To improve efficiency, we can sort the intervals and use a min-heap to manage eligible intervals efficiently. By sorting, we can leverage the order to quickly find and manage intervals that are relevant to each query.

### Detailed Explanation:
1. **Sort intervals and queries**: Sort both intervals by starting point and queries by value.
2. **Use a min-heap to store active intervals**: As we process each query, use a heap to keep track of the relevant intervals. The heap helps us to efficiently retrieve and remove intervals as they become irrelevant.
3. **Process each query**: For each query, add all starting intervals that include it to the heap. Remove any intervals that no longer cover the query.
4. **Select the smallest interval from the heap**: The top of the heap will give the smallest interval covering the current query.
5. **Store result**: If the heap is empty, store `-1`, else store the size of the interval at the top of the heap.

### Time Complexity:
- The sorting takes **O(n log n + m log m)** and processing is **O(n log n + m log n)**. This is more efficient than the naive approach for large inputs.

### Space Complexity:
- **O(n)** for the heap.

```typescript
function minIntervalSortingAndHeap(intervals: number[][], queries: number[]): number[] {
    // Sort intervals by their start, and queries by their value
    intervals.sort((a, b) => a[0] - b[0]);
    const sortedQueries = queries.map((q, i) => [q, i]).sort((a, b) => a[0] - b[0]);
    const result = new Array(queries.length);
    const minHeap = new MinPriorityQueue<{size: number, end: number}>();
    let index = 0;
    
    // Process each query
    for (let [query, originalIndex] of sortedQueries) {
        // Add all eligible intervals into the heap for the current query
        while (index < intervals.length && intervals[index][0] <= query) {
            const [start, end] = intervals[index];
            if (end >= query) {
                minHeap.enqueue({size: end - start + 1, end: end}, end - start + 1);
            }
            index++;
        }
        
        // Remove intervals that do not cover the current query
        while (!minHeap.isEmpty() && minHeap.front().element.end < query) {
            minHeap.dequeue();
        }
        
        // Determine the minimum interval size for this query
        result[originalIndex] = minHeap.isEmpty() ? -1 : minHeap.front().element.size;
    }
    
    return result;
}
```

In this code, use a `MinPriorityQueue` (this may require a priority queue library in TypeScript) to efficiently manage and retrieve intervals. The core idea is to maintain a running list of intervals that are potential candidates for each query via efficient management using a heap.

