# [Leetcode Problem 56: Merge Intervals](https://leetcode.com/problems/merge-intervals/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Sorting and Merging](#approach-2-sorting-and-merging)

### Approach 1: Brute Force

#### Intuition
A brute force approach to solve the problem involves comparing each interval with every other interval to determine overlaps. When two intervals overlap, we merge them into a new interval. The process is repeated until no more overlaps are found.

#### Code

```csharp
public IList<int[]> Merge(int[][] intervals) {
    if (intervals.Length <= 1)
        return intervals;

    // We'll store our results in this list
    List<int[]> result = new List<int[]>();
    
    // Create a set to keep track of merged intervals' indices to prevent duplicating merges
    HashSet<int> mergedIndices = new HashSet<int>();

    for (int i = 0; i < intervals.Length; i++) {
        if (mergedIndices.Contains(i))
            continue;
        
        int[] currentInterval = intervals[i];
        bool merged = false;
        
        for (int j = i + 1; j < intervals.Length; j++) {
            if (mergedIndices.Contains(j))
                continue;

            // Two intervals overlap if the start of one lies between the other
            if (intervals[j][0] <= currentInterval[1] && intervals[j][1] >= currentInterval[0]) {
                currentInterval[0] = Math.Min(currentInterval[0], intervals[j][0]);
                currentInterval[1] = Math.Max(currentInterval[1], intervals[j][1]);
                mergedIndices.Add(j);
                merged = true;
            }
        }
        
        // Add the current interval to results
        result.Add(currentInterval);
    }
    
    return result;
}
```

#### Time Complexity
- **O(n^2)** - In the worst case, each interval is compared with every other interval to check for overlap.

#### Space Complexity
- **O(n)** - The space complexity for storing merged intervals.

---

### Approach 2: Sorting and Merging

#### Intuition
By first sorting the intervals by their start times, the problem of merging becomes simpler. We iterate through the list and use a pointer to maintain the current interval. Whenever an overlap is detected, the current interval is expanded to include the overlapping interval. This method reduces the necessity of repeatedly comparing each interval with every other interval.

#### Code

```csharp
public IList<int[]> Merge(int[][] intervals) {
    if (intervals.Length <= 1)
        return intervals;

    // Sort intervals by their start time
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

    List<int[]> result = new List<int[]>();
    int[] currentInterval = intervals[0];
    result.Add(currentInterval);
    
    foreach (var interval in intervals) {
        // If the current interval's end overlaps with the next interval's start
        if (currentInterval[1] >= interval[0]) {
            // Merge the intervals by updating the end of the current interval
            currentInterval[1] = Math.Max(currentInterval[1], interval[1]);
        } else {
            // No overlap - move to the next interval
            currentInterval = interval;
            result.Add(currentInterval);
        }
    }
    
    return result;
}
```

#### Time Complexity
- **O(n log n)** - The intervals are sorted initially. The merging step is linear, O(n).

#### Space Complexity
- **O(n)** - The space complexity for returning the list of merged intervals.

This approach is efficient and represents the optimal solution to the problem. By sorting the intervals initially, we ensure that we only need a single pass through the intervals to achieve the desired merge result.

