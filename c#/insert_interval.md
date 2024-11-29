# 57. [Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approach: Iteration and Merge

### Solution
csharp
```csharp
// Time Complexity: O(n), where n is the number of intervals
// Space Complexity: O(n), for the result list
using System;
using System.Collections.Generic;

public class Solution {
    public int[][] Insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new List<int[]>();
        int i = 0, n = intervals.Length;

        // Step 1: Add all intervals that come before the new interval
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.Add(intervals[i]);
            i++;
        }

        // Step 2: Merge overlapping intervals with the new interval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.Min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.Max(newInterval[1], intervals[i][1]);
            i++;
        }
        result.Add(newInterval); // Add the merged interval

        // Step 3: Add all intervals that come after the new interval
        while (i < n) {
            result.Add(intervals[i]);
            i++;
        }

        // Convert the result list to a 2D array
        return result.ToArray();
    }
}
```

