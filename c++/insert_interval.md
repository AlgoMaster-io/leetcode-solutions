# 57. [Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approach: Iteration and Merge

### Solution
```cpp
// Time Complexity: O(n), where n is the number of intervals
// Space Complexity: O(n), for the result vector
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        int i = 0, n = intervals.size();

        // Step 1: Add all intervals that come before the new interval
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.push_back(intervals[i]);
            i++;
        }

        // Step 2: Merge overlapping intervals with the new interval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            i++;
        }
        result.push_back(newInterval); // Add the merged interval

        // Step 3: Add all intervals that come after the new interval
        while (i < n) {
            result.push_back(intervals[i]);
            i++;
        }

        return result;
    }
};
```

