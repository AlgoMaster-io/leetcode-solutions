# [Leetcode Problem 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Table of Contents
1. [Approach 1: Basic Linear Scan Insertion](#approach1)
2. [Approach 2: Optimal Merge Based Insertion](#approach2)

---

## Approach 1: Basic Linear Scan Insertion

### Intuition
The idea is to go through the intervals and find where the `newInterval` should fit. We do this by keeping track of whether we are before the new interval, within it, or past it. We can add all non-merged intervals to a result list and then merge the new interval where necessary.

### Implementation

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        
        int i = 0;
        int n = intervals.length;
        
        // Add all intervals before the newInterval
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.add(intervals[i]);
            i++;
        }
        
        // Merge intervals that overlap with the newInterval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }
        result.add(newInterval);  // Add the merged interval
        
        // Add remaining intervals
        while (i < n) {
            result.add(intervals[i]);
            i++;
        }
        
        return result.toArray(new int[result.size()][]);
    }
}
```

### Time Complexity
- **O(n):** We make a single pass over the intervals.
  
### Space Complexity
- **O(n):** We store the result in a new list which, in the worst case, could be as large as the input.

---

## Approach 2: Optimal Merge Based Insertion

### Intuition
While the first solution is efficient, this slightly optimized version ensures that we're minimizing operations on the intervals by directly placing them without additional checks once the current interval doesn't overlap. The core idea remains the same, making use of efficient merging and non-overlapping checks.

### Implementation

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        
        int i = 0;
        int n = intervals.length;
        
        // Iterate over all the intervals, adding them to the result as needed
        while (i < n) {
            if (intervals[i][1] < newInterval[0]) {
                // If the current interval ends before the new interval starts, add it
                result.add(intervals[i]);
            } else if (intervals[i][0] > newInterval[1]) {
                // If the current interval starts after the new interval ends, add the new interval and restart the process
                result.add(newInterval);
                newInterval = intervals[i];  // Move newInterval forward
            } else {
                // Intervals overlap, merge them
                newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
                newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            }
            i++;
        }
        
        // Add the last interval
        result.add(newInterval);
        
        return result.toArray(new int[result.size()][]);
    }
}
```

### Time Complexity
- **O(n):** Similar to the first approach, as each interval is visited once.

### Space Complexity
- **O(n):** Storing the resulting intervals in a list. 

This approach refines the merging logic slightly but achieves the same overall time complexity by structuring the logic around directly identifying when to place the `newInterval` and adjust it as necessary.

