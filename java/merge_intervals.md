# 56. [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Approach: Sort and Merge

### Solution
```java
// Time Complexity: O(n * log(n)), where n is the number of intervals (due to sorting)
// Space Complexity: O(n), for the result list
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;

public class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals == null || intervals.length == 0) {
            return new int[0][];
        }

        // Step 1: Sort intervals by their start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        // Step 2: Merge intervals
        List<int[]> merged = new ArrayList<>();
        int[] currentInterval = intervals[0];

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] <= currentInterval[1]) {
                // Overlapping intervals, merge them
                currentInterval[1] = Math.max(currentInterval[1], intervals[i][1]);
            } else {
                // Non-overlapping interval, add the current interval to the result
                merged.add(currentInterval);
                currentInterval = intervals[i];
            }
        }

        // Add the last interval
        merged.add(currentInterval);

        // Convert the result list to a 2D array
        return merged.toArray(new int[merged.size()][]);
    }
}
```