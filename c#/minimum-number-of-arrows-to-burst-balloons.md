# [Leetcode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approaches
- [Approach 1: Greedy Algorithm with Sorting](#approach-1-greedy-algorithm-with-sorting)

## Approach 1: Greedy Algorithm with Sorting

### Intuition
The problem of bursting balloons with the minimum number of arrows can be tackled with a greedy approach. The intuition is to minimize overlaps between the balloons by shooting the arrow through the most overlapping part of the balloons. The most efficient way to do this is by sorting the balloons based on their end coordinates. Once sorted, you can iterate through the list, and whenever you find a balloon whose start coordinate is greater than the current end coordinate of the trajectory, you shoot an arrow and update the trajectory. This ensures that each arrow burst as many balloons as possible, minimizing the total number of arrows.

### Detailed Steps:
1. Sort the balloons based on their end coordinates.
2. Iterate through each balloon and keep track of the number of arrows and the position of the last arrow fired.
3. For each balloon, if the start coordinate is greater than the end coordinate of the current trajectory (the end coordinate of the last balloon burst by the last arrow), fire a new arrow, and update this coordinate.

### Time Complexity
- **O(n log n)**: This is due to sorting the array of intervals.
  
### Space Complexity
- **O(1)**: If not considering the input storage, as no additional data structures proportional to the input size are used.

Here's the implementation in C#:

```csharp
public class Solution {
    public int FindMinArrowShots(int[][] points) {
        if (points.Length == 0) return 0;

        // Sort the balloons by their end coordinates
        Array.Sort(points, (a, b) => a[1].CompareTo(b[1]));

        // Initialize arrows with 1 since we need at least one arrow
        int arrows = 1;
        // The end of the first balloon in the sorted array
        int currentEnd = points[0][1];

        // Traverse through the sorted array
        for (int i = 1; i < points.Length; i++) {
            // If the start of the current balloon is greater than the end of 
            // the last balloon we covered, fire a new arrow
            if (points[i][0] > currentEnd) {
                arrows++;
                currentEnd = points[i][1]; // Update the end point for the new arrow
            }
            // Else, the balloon is already burst by the current arrow
        }

        return arrows;
    }
}
```

**Explanation with comments:**
- First, the intervals (representing balloons) are sorted by their end position.
- We initialize the `arrows` count to 1 since at least one arrow is needed.
- We also track `currentEnd` as the end of the first interval.
- As we iterate through the intervals, whenever a balloon's start point is further than `currentEnd`, we shoot a new arrow and update `currentEnd` to the end of the current balloon.
- This process ensures that each arrow burst the maximum number of overlapping balloons, minimizing the total number of arrows.

