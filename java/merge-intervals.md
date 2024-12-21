## [Leetcode 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

### Approaches:

1. [Brute Force - Comparing All Intervals](#approach-1-brute-force---comparing-all-intervals)
2. [Sort and Merge](#approach-2-sort-and-merge)

---

### Approach 1: Brute Force - Comparing All Intervals

#### Intuition:

The simplest, although not the most efficient, way to solve the problem is to compare each interval with every other interval to check for any overlapping. If two intervals overlap, they are merged into one. This process continues until we can no longer find any new intervals to merge, indicating all possible merges have been completed.

#### Steps:

1. Iterate over every interval and check with all other intervals whether they overlap.
2. If two intervals overlap, merge them and replace in the original list.
3. Continue this until no more merges can occur.

#### Code:

```java
import java.util.ArrayList;
import java.util.List;

public class MergeIntervals {
    public int[][] merge(int[][] intervals) {
        // We need to continue merging until no more merges are possible
        boolean hasMerged;
        do {
            hasMerged = false;
            List<int[]> merged = new ArrayList<>();
            for (int i = 0; i < intervals.length; i++) {
                boolean mergedCurrent = false;
                for (int j = 0; j < merged.size(); j++) {
                    // Check if intervals[i] overlaps with an interval in merged
                    if (merged.get(j)[1] >= intervals[i][0] && merged.get(j)[0] <= intervals[i][1]) {
                        // They overlap, merge them
                        merged.get(j)[0] = Math.min(merged.get(j)[0], intervals[i][0]);
                        merged.get(j)[1] = Math.max(merged.get(j)[1], intervals[i][1]);
                        hasMerged = true;
                        mergedCurrent = true;
                        break;
                    }
                }
                if (!mergedCurrent) {
                    // If current interval has not been merged, add it to the list
                    merged.add(intervals[i].clone());
                }
            }
            intervals = merged.toArray(new int[merged.size()][]);
        } while (hasMerged);
        return intervals;
    }
}
```

#### Complexity:

- **Time Complexity**: O(n^2), where n is the number of intervals. Each interval can potentially be compared with every other interval.
- **Space Complexity**: O(n), used for storing the merged intervals.

---

### Approach 2: Sort and Merge

#### Intuition:

A more optimal way to tackle the problem is to take advantage of sorting. By sorting intervals based on the start time, we can efficiently check for overlaps by comparing each interval with the last interval in a merged list. If they overlap, merge them; otherwise, just add the interval to the merged list.

#### Steps:

1. Sort the list of intervals based on the starting time.
2. Iterate through sorted intervals.
3. Compare the current interval with the last merged interval:
   - If they overlap, merge them.
   - If not, simply add the current interval to the result.

#### Code:

```java
import java.util.Arrays;
import java.util.LinkedList;

public class MergeIntervals {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }
        
        // Sort the intervals by their start time
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        
        // Use a LinkedList to hold the merged intervals
        LinkedList<int[]> merged = new LinkedList<>();
        
        // Iterate through the sorted intervals
        for (int[] interval : intervals) {
            // If merged list is empty or no overlap with the last interval in merged
            if (merged.isEmpty() || merged.getLast()[1] < interval[0]) {
                merged.add(interval);
            } else {
                // If there is an overlap, merge the current interval with the last one
                merged.getLast()[1] = Math.max(merged.getLast()[1], interval[1]);
            }
        }
        
        return merged.toArray(new int[merged.size()][]);
    }
}
```

#### Complexity:

- **Time Complexity**: O(n log n), where n is the number of intervals. Sorting the intervals dominates the time complexity.
- **Space Complexity**: O(n), used for storing the merged intervals.

Both approaches solve the problem of merging intervals, but the second one is more efficient, exploiting sorting to reduce unnecessary comparisons.

