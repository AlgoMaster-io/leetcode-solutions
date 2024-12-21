# [Leetcode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approaches
- [Greedy Overlapping Intervals](#greedy-overlapping-intervals)

---
### Greedy Overlapping Intervals

#### Intuition
The problem can be related to the "Interval Scheduling Maximization" or "Interval Partition" problem. The main idea is to group overlapping intervals (or balloons) to minimize the number of arrows required. We can sort the balloons by their end position and keep track of the latest arrow position used to burst overlapping balloons. Each time a non-overlapping interval is found, we increment the arrow count and update the latest arrow position.

#### Approach
1. **Sort the Intervals**: First, sort the balloon intervals by their ending positions. This helps us efficiently find non-overlapping intervals when comparing intervals.
2. **Iterate Through Sorted Balloons**:
   - Initialize `arrowCount` to 0 and `arrowPos` to negative infinity to indicate that no arrows have been used yet.
   - Traverse through the sorted intervals.
     - If the start position of the current balloon is greater than `arrowPos`, it means a new arrow is needed, as the current balloon starts outside the range of the last positioned arrow.
     - Increment the `arrowCount` and update the `arrowPos` to the end position of the current balloon.
3. Return the `arrowCount` as it represents the minimum number of arrows required.

#### Code
```javascript
/**
 * @param {number[][]} points
 * @return {number}
 */
const findMinArrowShots = function(points) {
    // Sort the balloons based on their end positions
    points.sort((a, b) => a[1] - b[1]);
    
    let arrowCount = 0;
    let arrowPos = -Infinity;
    
    for (let i = 0; i < points.length; i++) {
        // If the current balloon starts after the last arrow position,
        // this means a new arrow is necessary.
        if (arrowPos < points[i][0]) {
            arrowCount++;
            // Set the position of the new arrow to the end of the current balloon
            arrowPos = points[i][1];
        }
    }
    
    return arrowCount;
};
```

#### Detailed Steps
- **Initialization**: Start with `arrowCount` as 0 indicating no arrows have been used. Use `arrowPos` as negative infinity to represent that no arrow has been placed.
- **Sorting**: The input `points` array is sorted by each interval's end value to help identify the minimal overlap solution.
- **Looping and Counting**:
  - The loop iterates through each interval.
  - If an interval starts after the last placed arrow position (`arrowPos`), increment the `arrowCount` and shift the `arrowPos` to the current interval's end.

#### Complexity
- **Time Complexity**: \(O(n \log n)\), where \(n\) is the number of balloons, due to sorting.
- **Space Complexity**: \(O(1)\), since no additional space proportional to input size is used outside of sorting operations.

