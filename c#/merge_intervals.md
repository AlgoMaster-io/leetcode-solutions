# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
csharp
```csharp
// Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
// Space Complexity: O(n), for the result list
using System;
using System.Collections.Generic;

public class Solution {
    public int[][] Merge(int[][] intervals) {
        if (intervals == null || intervals.Length == 0) {
            return new int[0][];
        }

        // Step 1: Sort intervals by their start time
        Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

        // Step 2: Merge intervals
        List<int[]> merged = new List<int[]>();
        int[] currentInterval = intervals[0];

        for (int i = 1; i < intervals.Length; i++) {
            if (intervals[i][0] <= currentInterval[1]) {
                // Overlapping intervals, merge them
                currentInterval[1] = Math.Max(currentInterval[1], intervals[i][1]);
            } else {
                // Non-overlapping interval, add the current interval to the result
                merged.Add(currentInterval);
                currentInterval = intervals[i];
            }
        }

        // Add the last interval
        merged.Add(currentInterval);

        // Convert the result list to a 2D array
        return merged.ToArray();
    }
}
```

