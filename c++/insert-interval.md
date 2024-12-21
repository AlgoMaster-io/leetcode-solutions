# [Leetcode 57: Insert Interval](https://leetcode.com/problems/insert-interval/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Efficient Merging Approach](#efficient-merging-approach)

## Brute Force Approach

### Intuition:
The brute force approach involves simply inserting the new interval into the list of intervals and then sorting the list. After sorting, we then iterate through the intervals and merge them if they overlap. This approach is straightforward but not very efficient.

### Steps:
1. Insert the new interval into the intervals list.
2. Sort the list based on the start times of the intervals.
3. Iterate through the sorted intervals and merge them if they overlap.
4. Maintain a result list to store non-overlapping intervals.

### Code:
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        // Step 1: Insert the new interval into the intervals list
        intervals.push_back(newInterval);
        
        // Step 2: Sort the list based on the start times of the intervals
        sort(intervals.begin(), intervals.end());
        
        vector<vector<int>> result;
        
        for (auto& interval : intervals) {
            // If the result is empty or the current interval does not overlap with the last interval in result
            if (result.empty() || result.back()[1] < interval[0]) {
                result.push_back(interval);
            } else {
                // Overlapping intervals, merge them by updating the end time
                result.back()[1] = max(result.back()[1], interval[1]);
            }
        }
        
        return result;
    }
};
```

### Time Complexity:
- **O(n log n)** due to sorting the intervals list.
- **O(n)** for iterating through the intervals to merge them.

### Space Complexity:
- **O(n)** for the result list to store merged intervals.

## Efficient Merging Approach

### Intuition:
Instead of sorting the intervals, we can directly merge the new interval with the existing list by iterating through it once and placing the new interval in the correct position. This approach skips the sorting step, leveraging the fact that the original list is sorted.

### Steps:
1. Initialize a result list.
2. Iterate over the existing intervals:
   - Add all the intervals that end before the new interval starts.
   - Merge all overlapping intervals with the new interval.
   - Add all remaining intervals after the merged interval.
3. Handle edge cases such as an empty list or the new interval not overlapping with any existing intervals.

### Code:
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        int i = 0;
        int n = intervals.size();
        
        // Step 1: Add all intervals that come before the new interval
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.push_back(intervals[i++]);
        }
        
        // Step 2: Merge overlapping intervals with the new interval
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]); // Update newInterval start
            newInterval[1] = max(newInterval[1], intervals[i][1]); // Update newInterval end
            i++;
        }
        result.push_back(newInterval); // Add the merged interval
        
        // Step 3: Add all the remaining intervals
        while (i < n) {
            result.push_back(intervals[i++]);
        }
        
        return result;
    }
};
```

### Time Complexity:
- **O(n)** as we iterate through the intervals once.

### Space Complexity:
- **O(n)** for the result list to store merged intervals. 

This more efficient approach results in a cleaner solution with lower time complexity due to the elimination of the sorting step.

