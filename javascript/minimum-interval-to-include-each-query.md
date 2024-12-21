## [Leetcode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sort and Use a Min-Heap](#approach-2-sort-and-use-a-min-heap)

---

### Approach 1: Brute Force

**Intuition:**

The brute force approach involves iterating over each query and for each query, checking all the intervals to find the smallest interval that contains the query. For each interval, we check if it includes the query and keep track of the smallest such interval.

**Steps:**
1. For each query, iterate through all the intervals.
2. Check if the interval contains the query.
3. If it does, calculate the size of the interval. Keep track of the smallest interval size for each query.
4. If no interval is found for a query, return -1 for that query.

**Time Complexity:** O(Q * N), where Q is the number of queries and N is the number of intervals.  
**Space Complexity:** O(1), as we do not use any extra space besides the input and output.

```javascript
function minInterval(intervals, queries) {
    const results = [];
    
    // Iterate over each query
    for (const query of queries) {
        let minSize = Infinity;
        
        // Check each interval
        for (const [start, end] of intervals) {
            // If the query is within the interval
            if (start <= query && query <= end) {
                // Calculate interval size
                const size = end - start + 1;
                // Update minimum size if the current one is smaller
                minSize = Math.min(minSize, size);
            }
        }
        
        // If minSize is not updated, return -1, else return minSize
        results.push(minSize === Infinity ? -1 : minSize);
    }
    
    return results;
}
```

---

### Approach 2: Sort and Use a Min-Heap

**Intuition:**

This method employs sorting and a min-heap (priority queue) to efficiently find the smallest interval for each query:
1. **Sort intervals and queries.** We sort the intervals by the starting point and queries by value which helps in efficiently finding the candidate intervals for each query.
2. **Use a min-heap to maintain active intervals.** As we process each query, we use a min-heap to maintain intervals whose end points have not been passed. These are candidates for the current query.
3. **For each query, add eligible intervals to the heap** (interval starts before or when the query happens).
4. **Remove intervals from the heap** that end before the current query.
5. The smallest interval containing the query will be on top of the min-heap as we remove non-viable entries.

**Steps:**
1. Sort intervals by the starting point and queries with their indices.
2. Use a min-heap to store the size of intervals. The heap will maintain the smallest interval to the top.
3. As we move through sorted queries, add intervals to the heap and remove expired intervals.
4. For each query, extract the minimum-sized interval from the heap that contains the query.

**Time Complexity:** O((N + Q) log N), where N is the number of intervals and Q is the number of queries.  
**Space Complexity:** O(N), for storing intervals in the heap.

```javascript
function minInterval(intervals, queries) {
    // Sort intervals by start, if same start then by end
    intervals.sort((a, b) => a[0] - b[0]);
    
    // Sort queries keeping original index
    const indexedQueries = queries.map((query, index) => [query, index]);
    indexedQueries.sort((a, b) => a[0] - b[0]);

    const minHeap = []; // Min-heap to store active intervals by size
    const results = Array(queries.length).fill(-1);
    let i = 0; // Index for intervals

    // Go through each query in sorted order
    for (let [query, index] of indexedQueries) {
        // Add all intervals that start before or when this query happens
        while (i < intervals.length && intervals[i][0] <= query) {
            const [start, end] = intervals[i];
            const size = end - start + 1;
            // Add interval to heap
            minHeap.push([size, end]);
            // Reorder minHeap based on size smallest at top
            minHeap.sort((a, b) => a[0] - b[0]);
            i++;
        }

        // Remove from heap intervals that closed before this query
        while (minHeap.length > 0 && minHeap[0][1] < query) {
            minHeap.shift();
        }

        // Top of the heap is the smallest interval that can cover this query if it exists
        if (minHeap.length > 0) {
            results[index] = minHeap[0][0];
        }
    }

    return results;
}
```

This approach efficiently finds the minimum interval for each query by leveraging sorting and a min-heap to track active intervals dynamically.

