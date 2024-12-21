[Leetcode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting with Min-Heap](#approach-2-sorting-with-min-heap)

### Approach 1: Brute Force

#### Intuition
The basic idea for the brute force approach is to iterate through each query and check every interval to find the smallest interval that includes the query. While this approach is straightforward, it is not efficient due to the nested loops which result in high computational complexity.

#### Solution
```python
def minInterval(intervals, queries):
    result = []
    for query in queries:
        min_size = float('inf')
        for start, end in intervals:
            if start <= query <= end:
                min_size = min(min_size, end - start + 1)
        result.append(min_size if min_size != float('inf') else -1)
    return result

# Time Complexity: O(n * m) where n is the number of intervals and m is the number of queries.
# Space Complexity: O(1) if excluding the output storage.
```

### Approach 2: Sorting with Min-Heap

#### Intuition
A more optimal approach involves sorting the intervals by their starting point and the queries. By iterating through the sorted intervals and maintaining a min-heap ordered by interval length, we can efficiently find the smallest interval that includes each query. This method avoids redundant comparisons and uses a heap to quickly access the minimum intervals.

#### Solution
```python
import heapq

def minInterval(intervals, queries):
    # Sort the intervals by start time
    intervals.sort()
    # Pair queries with their original indices
    queries = sorted((q, i) for i, q in enumerate(queries))
    # The result placeholder
    result = [-1] * len(queries)
    # Min-Heap to store active intervals
    min_heap = []
    
    # Index for intervals
    idx = 0
    # Iterate through each query in sorted order
    for q, i in queries:
        # Add all intervals that start <= current query to the heap
        while idx < len(intervals) and intervals[idx][0] <= q:
            start, end = intervals[idx]
            # Only consider the intervals that can include the current query
            if end >= q:
                heapq.heappush(min_heap, (end - start + 1, end))
            idx += 1
        
        # Remove intervals from the heap that cannot include the current query
        while min_heap and min_heap[0][1] < q:
            heapq.heappop(min_heap)
        
        # If the heap still has elements, the top has the smallest interval for the query
        if min_heap:
            result[i] = min_heap[0][0]
    
    return result

# Time Complexity: O((n + m) log n) where n is the number of intervals and m is the number of queries.
# Space Complexity: O(n + m) for sorting and heap storage.
```

This solution leverages sorting and a min-heap to significantly reduce the number of operations compared to the brute force approach, making it more efficient for larger inputs.

