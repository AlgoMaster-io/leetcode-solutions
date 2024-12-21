# [LeetCode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using Sorting and Min-Heap](#optimized-approach-using-sorting-and-min-heap)

### Brute Force Approach

#### Intuition
The basic idea is to iterate through each query and check all the intervals to determine the smallest interval that includes it. This is a direct approach and can become inefficient when dealing with larger input sizes since we are examining each interval for each query.

#### Steps:
1. For each query, iterate through all intervals and check if the interval includes the query.
2. Calculate the interval size if the query is included and keep track of the smallest interval found.
3. If an interval does not include the query, skip it.
4. If at the end of checking all intervals no covering interval was found, return a suitable default (e.g., -1).

#### Time Complexity
- **O(m \* n)** where `m` is the number of queries and `n` is the number of intervals. This is because we are checking each query against all intervals linearly.

#### Space Complexity
- **O(1)** additional space is utilized.

```csharp
public int[] MinInterval(int[][] intervals, int[] queries) {
    int[] result = new int[queries.Length];
    for (int i = 0; i < queries.Length; i++) {
        int minIntervalSize = int.MaxValue;
        foreach (var interval in intervals) {
            // Check if the query falls within the interval
            if (interval[0] <= queries[i] && interval[1] >= queries[i]) {
                // Calculate the size of the interval
                int size = interval[1] - interval[0] + 1;
                // Update the minimum interval size found
                if (size < minIntervalSize) {
                    minIntervalSize = size;
                }
            }
        }
        // Store the result for the current query
        result[i] = minIntervalSize == int.MaxValue ? -1 : minIntervalSize;
    }
    return result;
}
```

### Optimized Approach Using Sorting and Min-Heap

#### Intuition
This approach takes advantage of sorting and using a min-heap to efficiently track the smallest interval. The main idea is to:
1. Sort both the intervals and the queries.
2. Use a min-heap to store intervals as they become relevant to the current query, allowing us to quickly find the smallest interval.
3. Process the queries in ascending order and manage relevant intervals dynamically using the heap.

#### Steps:
1. Sort intervals by their starting point.
2. Sort queries and keep track of their original indices.
3. Use a priority queue (min-heap) to store intervals, sorted by their size.
4. For each query, add all intervals starting before the query to the heap.
5. Remove intervals from the heap that cannot include the query (i.e., their end is less than the query).
6. The top of the heap will give the minimum interval that includes the query (if the heap is not empty).

#### Time Complexity
- **O(n log n + m log m + (m + n) log n)**: Sorting intervals and queries takes `O(n log n + m log m)`, and maintaining the heap operations is amortized to `O((m + n) log n)`, given that each interval and query can be processed at most once.

#### Space Complexity
- **O(m + n)**: Space for storing the query indices and the heap.

```csharp
public int[] MinInterval(int[][] intervals, int[] queries) {
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));
    var indexedQueries = queries.Select((q, index) => new { Query = q, Index = index }).OrderBy(q => q.Query).ToArray();
    
    int[] result = new int[queries.Length];
    PriorityQueue<int[], int> minHeap = new PriorityQueue<int[], int>();
    int intervalIndex = 0;
    
    foreach (var qInfo in indexedQueries) {
        int query = qInfo.Query;
        int queryIndex = qInfo.Index;
        
        // Add all intervals where start <= current query to the heap
        while (intervalIndex < intervals.Length && intervals[intervalIndex][0] <= query) {
            int start = intervals[intervalIndex][0];
            int end = intervals[intervalIndex][1];
            int size = end - start + 1;
            minHeap.Enqueue(new int[] { end, size }, size);
            intervalIndex++;
        }
        
        // Remove intervals from the heap that end before the current query
        while (minHeap.Count > 0 && minHeap.Peek()[0] < query) {
            minHeap.Dequeue();
        }
        
        // The smallest interval that includes the current query is at the top of the heap
        result[queryIndex] = minHeap.Count > 0 ? minHeap.Peek()[1] : -1;
    }
    
    return result;
}
```

This approach uses a combination of sorting and a min-heap to efficiently determine the smallest interval for each query. It is optimal given the constraints of the problem.


