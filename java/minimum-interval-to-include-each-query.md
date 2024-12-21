# [Leetcode 1851: Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sorting and Two Pointers with Priority Queue](#approach-2-sorting-and-two-pointers)

### Approach 1: Brute Force

#### Intuition
In the brute force approach, we will iterate over each query and for each query, traverse through all the intervals to check which interval can contain the query, then select the smallest interval among those that can accommodate the query.

#### Steps
1. For each query, iterate through each interval.
2. For each query, find all intervals [start, end] where `start <= query <= end`.
3. Out of these suitable intervals, select the interval of minimum length `(end - start + 1)`.
4. Store the result for each query.

#### Code
```java
class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        int n = queries.length;
        int[] result = new int[n];
        Arrays.fill(result, -1); // Initialize result with -1
        
        // For each query
        for (int i = 0; i < n; i++) {
            int query = queries[i];
            int minIntervalSize = Integer.MAX_VALUE;
            
            // Check each interval
            for (int[] interval : intervals) {
                int start = interval[0];
                int end = interval[1];
                
                if (start <= query && query <= end) {
                    int intervalSize = end - start + 1;
                    minIntervalSize = Math.min(minIntervalSize, intervalSize);
                }
            }
            
            if (minIntervalSize != Integer.MAX_VALUE) {
                result[i] = minIntervalSize;
            }
        }
        
        return result;
    }
}
```

#### Time Complexity
- O(n * m), where n is the number of queries and m is the number of intervals.

#### Space Complexity
- O(1), as we are using a fixed amount of space regardless of input size.

---

### Approach 2: Sorting and Two Pointers with Priority Queue

#### Intuition
The brute force solution is not efficient enough for larger inputs. To optimize, we can use a combination of sorting and priority queue:
- Sort the intervals and queries.
- Use a priority queue to keep track of interval sizes as we move through the intervals and handle queries.

#### Steps
1. Sort intervals by their start points.
2. Sort queries along with their indices so that we can retrieve the result in original order.
3. Use a min-heap (priority queue) to store intervals based on their end points while processing queries.
4. For each query, filter intervals that can cover the query and add them to the priority queue.
5. Remove the intervals from the priority queue which can no longer cover future queries.
6. The smallest element in the priority queue will be the minimum interval for the current query.

#### Code
```java
import java.util.*;

class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        // Prepare result array
        int n = queries.length;
        int[] result = new int[n];
        
        // Pair of query and its index
        List<int[]> sortedQueries = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            sortedQueries.add(new int[]{queries[i], i});
        }
        
        // Sort intervals by start and queries by value
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
        sortedQueries.sort(Comparator.comparingInt(a -> a[0]));
        
        // Min heap based on interval size
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        
        int i = 0; // Pointer for intervals
        for (int[] query : sortedQueries) {
            int q = query[0];
            int index = query[1];
            
            // Add all intervals that can start before the current query
            while (i < intervals.length && intervals[i][0] <= q) {
                int start = intervals[i][0];
                int end = intervals[i][1];
                int size = end - start + 1;
                minHeap.offer(new int[]{end, size});
                i++;
            }
            
            // Remove all intervals from heap where end is smaller than the current query
            while (!minHeap.isEmpty() && minHeap.peek()[0] < q) {
                minHeap.poll();
            }
            
            // The top of the heap is the smallest interval containing the query
            if (!minHeap.isEmpty()) {
                result[index] = minHeap.peek()[1];
            } else {
                result[index] = -1;
            }
        }

        return result;
    }
}
```

#### Time Complexity
- O((m + n) log m), where n is the number of queries and m is the number of intervals. This includes sorting intervals, queries and operations related to priority queue.

#### Space Complexity
- O(m), for storing intervals that are currently being considered in the priority queue.

