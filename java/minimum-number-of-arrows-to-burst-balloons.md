# [Leetcode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approaches:

- [Approach 1: Sorting and Greedy Approach](#approach-1-sorting-and-greedy-approach)

---

## Approach 1: Sorting and Greedy Approach

### Intuition

The problem can be visualized as an interval overlap problem. Each balloon can be represented by an interval (start, end) based on its diameter. The goal is to find the minimum number of arrows required to burst all balloons, which translates to finding the minimum number of non-overlapping intervals required to cover all the intervals. 

A greedy approach is useful here:
1. Sort the intervals by their ending values.
2. Start with the first interval and shoot an arrow at its end. This arrow will cover all balloons that start before this end.
3. Move to the next interval that starts after the current arrow position. Repeat until all balloons are covered.

### Algorithm

1. **Sort**: Sort the intervals by their end values in ascending order.
2. **Initialize**: Set the current arrow position to the end of the first interval and start counting arrows.
3. **Iterate**: Go through each balloon interval:
   - If the start of the current balloon is greater than the current arrow position, shoot a new arrow at the end of this current balloon.
   - Update the current arrow position to this new end.

### Time and Space Complexity

- **Time Complexity**: O(n log n), where n is the number of balloons. This comes mainly from sorting the intervals.
- **Space Complexity**: O(1), aside from input storage since we are sorting in place.

```java
import java.util.Arrays;

public class MinimumArrowsToBurstBalloons {
    public int findMinArrowShots(int[][] points) {
        // Sorting the balloon intervals by their end position
        Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));

        int arrows = 0;
        Integer currentArrowPosition = null;

        for (int[] balloon : points) {
            // If currentArrowPosition is null or if the current balloon starts after the last arrow
            if (currentArrowPosition == null || balloon[0] > currentArrowPosition) {
                // Need to use a new arrow, so increase the arrow count
                arrows++;
                // Update the current arrow position to the end of this balloon
                currentArrowPosition = balloon[1];
            }
            // Else, the current balloon is within the range of the last arrow so no need for a new one
        }

        return arrows;
    }

    public static void main(String[] args) {
        MinimumArrowsToBurstBalloons solution = new MinimumArrowsToBurstBalloons();
        int[][] points = {{10,16}, {2,8}, {1,6}, {7,12}};
        System.out.println(solution.findMinArrowShots(points));  // Output: 2
    }
}
```

### Detailed Comments
- **Line 5-6 (Sorting)**: Sort the balloons based on the end of their intervals. This allows us to always shoot an arrow that will cover the maximum number of rays by extending our range to the farthest right end first.
- **Lines 9-10 (Initialization)**: Initialize the number of arrows needed and set the current arrow position as `null` initially.
- **Line 12-20 (Iterate over intervals)**: For each balloon, check if it starts after the last arrow's position, if yes, shoot a new arrow at the end of the current balloon interval to cover as many upcoming balloons as possible.

This approach uses a greedy strategy to minimize the number of arrows needed by making optimal 'local' decisions.

