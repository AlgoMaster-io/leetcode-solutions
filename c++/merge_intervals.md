# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
```cpp
// Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
// Space Complexity: O(n), for the result vector
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if (intervals.empty()) {
            return {};
        }

        // Step 1: Sort intervals by their start time
        sort(intervals.begin(), intervals.end());

        // Step 2: Merge intervals
        vector<vector<int>> merged;
        vector<int> currentInterval = intervals[0];

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] <= currentInterval[1]) {
                // Overlapping intervals, merge them
                currentInterval[1] = max(currentInterval[1], intervals[i][1]);
            } else {
                // Non-overlapping interval, add the current interval to the result
                merged.push_back(currentInterval);
                currentInterval = intervals[i];
            }
        }

        // Add the last interval
        merged.push_back(currentInterval);

        return merged;
    }
};
```

