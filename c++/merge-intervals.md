# Leetcode Problem: [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approaches
- [Approach 1: Brute Force Approach](#approach-1-brute-force-approach)
- [Approach 2: Sort and Merge](#approach-2-sort-and-merge)

---

## Approach 1: Brute Force Approach

### Intuition
The basic idea is to compare each interval with every other interval to check if they overlap, and if they do, merge them into one. This is the most straightforward way to solve the problem but is computationally expensive due to its high time complexity.

### Steps
1. Use two nested loops to compare each interval with every other interval.
2. For each pair of intervals, check if they overlap.
3. If they overlap, merge the intervals into one and replace the original intervals with this merged interval.
4. Continue the process until no more intervals can be merged.

### Code
```cpp
#include <vector>
using namespace std;

vector<vector<int>> merge(vector<vector<int>>& intervals) {
    // Resulting merged intervals
    vector<vector<int>> merged;
    int n = intervals.size();
    vector<bool> mergedFlags(n, false);

    for (int i = 0; i < n; ++i) {
        if (mergedFlags[i]) continue;

        int start = intervals[i][0];
        int end = intervals[i][1];
        for (int j = i + 1; j < n; ++j) {
            // Check if intervals[i] overlaps with intervals[j]
            if (!(end < intervals[j][0] || start > intervals[j][1])) {
                // Merge intervals[i] and intervals[j]
                start = min(start, intervals[j][0]);
                end = max(end, intervals[j][1]);
                mergedFlags[j] = true; // Mark as merged
            }
        }
        merged.push_back({start, end});
    }
    return merged;
}
```

### Time Complexity
- **Time Complexity**: \(O(n^2)\), as each interval is compared with every other interval.
- **Space Complexity**: \(O(n)\), for storing the merged result.

---

## Approach 2: Sort and Merge

### Intuition
By sorting the intervals based on their starting time, overlapping intervals will be adjacent to each other. Thus, we can iterate through the sorted list and merge intervals if they overlap.

### Steps
1. Sort the intervals based on their starting times.
2. Initialize an empty list to hold the merged intervals.
3. Iterate through the sorted intervals:
    - Compare the current interval with the last merged interval.
    - If they overlap, merge them by updating the end time of the last interval added.
    - If they do not overlap, add the current interval to the list.
4. Return the merged list of intervals.

### Code
```cpp
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};

    // Sort intervals based on their start time
    sort(intervals.begin(), intervals.end());

    // Merged intervals result
    vector<vector<int>> merged;
    // Start with the first interval
    merged.push_back(intervals[0]);

    for (int i = 1; i < intervals.size(); ++i) {
        // Get the last interval from the merged list
        vector<int>& last = merged.back();

        // Check if the current interval can be merged with the last interval
        if (intervals[i][0] <= last[1]) {
            // Merge them by updating the end time
            last[1] = max(last[1], intervals[i][1]);
        } else {
            // Otherwise, add the current interval to the merged list
            merged.push_back(intervals[i]);
        }
    }

    return merged;
}
```

### Time Complexity
- **Time Complexity**: \(O(n \log n)\), due to the sorting of intervals.
- **Space Complexity**: \(O(n)\), for storing the merged result.

---

By following these approaches, you can solve this problem starting from a simpler brute force method to a more efficient method leveraging sorting.

