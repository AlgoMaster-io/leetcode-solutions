# Leetcode Problem: [Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sort + Min-Heap](#approach-2-sort--min-heap)

### Approach 1: Brute Force

#### Intuition:
The brute force approach involves checking each query with every given interval to determine the minimum interval that can cover the query. This approach directly evaluates each possibility but can be highly inefficient for large input sizes.

#### Solution:

```cpp
#include <vector>
#include <climits>
using namespace std;

vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
    int n = queries.size();
    vector<int> result(n, INT_MAX);
    
    // Iterate over each query
    for (int i = 0; i < n; ++i) {
        int query = queries[i];
        int minSize = INT_MAX;
        
        // Check every interval
        for (const auto& interval : intervals) {
            int start = interval[0];
            int end = interval[1];
            
            // Check if query is within the interval
            if (start <= query && query <= end) {
                minSize = min(minSize, end - start + 1);
            }
        }
        
        // If minSize remained unchanged, update result with -1
        result[i] = (minSize == INT_MAX) ? -1 : minSize;
    }
    
    return result;
}
```

#### Time Complexity:
- O(m * n) where `m` is the number of intervals and `n` is the number of queries. We compare each query with each interval.

#### Space Complexity:
- O(1) extra space, aside from input and output, as we only use variables for calculations.

---

### Approach 2: Sort + Min-Heap

#### Intuition:
A more optimal solution utilizes sorting and a min-heap (priority queue). By sorting intervals by start time and queries by value, we can efficiently process intervals as we iterate through sorted queries. A min-heap helps keep track of the smallest valid interval for each query.

#### Solution:

```cpp
#include <vector>
#include <algorithm>
#include <queue>
using namespace std;

vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
    // Sort intervals based on start time
    sort(intervals.begin(), intervals.end());
    
    // Prepare query structure holding original indices
    int q = queries.size();
    vector<pair<int, int>> sortedQueries;
    for (int i = 0; i < q; ++i) {
        sortedQueries.emplace_back(queries[i], i);
    }
    
    // Sort queries based on query values
    sort(sortedQueries.begin(), sortedQueries.end());
    
    vector<int> result(q, -1);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> minHeap;
    int i = 0; // Interval index
    
    // Process each query in sorted order
    for (const auto& [query, index] : sortedQueries) {
        // Push intervals that can start before or at the current query
        while (i < intervals.size() && intervals[i][0] <= query) {
            int start = intervals[i][0];
            int end = intervals[i][1];
            int size = end - start + 1;
            minHeap.emplace(size, end);
            i++;
        }
        
        // Remove intervals from the heap that cannot cover the current query
        while (!minHeap.empty() && minHeap.top().second < query) {
            minHeap.pop();
        }
        
        // The top of the heap is the smallest valid interval covering the query
        if (!minHeap.empty()) {
            result[index] = minHeap.top().first;
        }
    }
    
    return result;
}
```

#### Time Complexity:
- O(m log m + n log n + (m+n) log m) where `m` is the number of intervals and `n` is the number of queries. Sorting intervals, sorting queries, and heap operations contribute to the complexity.

#### Space Complexity:
- O(m), primarily due to the space required for the heap storing active intervals.

