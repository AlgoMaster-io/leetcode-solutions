# [Leetcode 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Merged Insert and Build Approach](#merged-insert-and-build-approach)
3. [Optimal Merge-Based Approach](#optimal-merge-based-approach)


### Brute Force Approach
#### Intuition
The problem of inserting an interval into a list of non-overlapping intervals can initially be tackled by simply iterating through each interval and determining where and how to insert the new interval.

1. **Identify the Point of Insertion:**
   - Traverse the given list of intervals to find the correct position to insert the new interval without considering merging.

2. **Merge Overlapping:**
   - After inserting, check each interval with respect to the recently added interval for overlaps and merge them if necessary.

#### Implementation
```csharp
public int[][] Insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new List<int[]>();

    // Check if there's any interval overlapping, keep adding and merging if possible
    foreach (var interval in intervals) {
        // If the current interval ends before the new interval starts
        if (interval[1] < newInterval[0]) {
            result.Add(interval);
        }
        // If the new interval ends before the current interval starts
        else if (newInterval[1] < interval[0]) {
            result.Add(newInterval);
            newInterval = interval;
        }
        // Overlapping intervals, merge them
        else {
            newInterval[0] = Math.Min(newInterval[0], interval[0]);
            newInterval[1] = Math.Max(newInterval[1], interval[1]);
        }
    }

    result.Add(newInterval);
    return result.ToArray();
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n)\), where \(n\) is the number of intervals. Each interval is processed once.
- **Space Complexity:** \(O(n)\), the space used by the result to store the new set of intervals.

---
### Merged Insert and Build Approach
#### Intuition
To improve efficiency, recognize that we can build the result list during a single pass over the intervals like before, but doing it systematically without needing a manually handled insertion:

1. **Before Overlap:**
   - Add all intervals before we encounter the overlap scenario.

2. **During Overlap:**
   - Merge all intervals that overlap with the new interval.

3. **After Overlap:**
   - Add the remaining intervals as they are.

This allows us to control each step explicitly by deciding what happens under all end and start comparisons.

#### Implementation
```csharp
public int[][] Insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new List<int[]>();
    int i = 0;
    int n = intervals.Length;

    // Add all intervals that come before the newInterval
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.Add(intervals[i]);
        i++;
    }
    
    // Merge all overlapping intervals with newInterval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.Min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.Max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.Add(newInterval);
    
    // Add the remaining intervals
    while (i < n) {
        result.Add(intervals[i]);
        i++;
    }
    
    return result.ToArray();
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n)\), traversing n intervals.
- **Space Complexity:** \(O(n)\), result list is expected to manage maximum possible intervals after merge.

---
### Optimal Merge-Based Approach
#### Intuition
We can visualize the process more structure-wise from start to end by focusing strictly on avoiding any operations beyond necessary conditions following our three-step process (before-overlap-after):

1. **Optimal Point:**
   - Determine insertions by skipping non-relevant intervals before ensuring no extra calculations than before.

2. **Efficient Merging:**
   - Seamlessly modifying and keep scanning merged spans unless overlapping is settled.

3. **Comprehensively Conclude:**
   - Divert direct path through remaining steps to conclude elegance after amalgamation decides the diligent collected set.

#### Implementation
```csharp
public int[][] Insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new List<int[]>();
    int i = 0;
    int n = intervals.Length;

    // Adding intervals before any potential overlap
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.Add(intervals[i]);
        i++;
    }
    
    // Resolve overlapping
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.Min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.Max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.Add(newInterval); 
    
    // After overlaps are resolved, rest intervals
    while (i < n) {
        result.Add(intervals[i]);
        i++;
    }
    
    return result.ToArray();
}
```

#### Complexity Analysis
- **Time Complexity:** \(O(n)\) strictly per interval scan pass in precise overlap delineations.
- **Space Complexity:** \(O(n)\) ensuing interval outputs recorded proportionality of structural variations along merge completions.

This approach optimally captures the insert by leveraging single subset recognition without overhand energy beyond linear passage integrities aligned.

