# [Leetcode Problem 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approaches
1. [Greedy Approach with Sorting](#greedy-approach-with-sorting)

---

## Greedy Approach with Sorting

### Intuition
The problem can be visualized as having intervals, where each balloon has a start and an end point on the x-axis. The task is to find the minimum number of arrows that can burst all the balloons, where one arrow can burst all balloons that are overlapping at a particular point.

The optimal strategy is to always pick the interval (balloon) that finishes earliest. By sorting the balloons by their end coordinates, we can check each balloon and determine whether it can be burst with the current arrow, or if we need an additional arrow.

1. **Step 1:** First, we need to sort all the balloons by their end coordinate.
2. **Step 2:** Start with the first balloon's end as the position of our first arrow.
3. **Step 3:** Iterate through each balloon and check if it starts after the arrow's position. If it does, we need an additional arrow, placed at this balloon's end position.

### Algorithm
1. Sort the balloons based on their end coordinates.
2. Initialize the count of arrows needed to 1 (as we need at least one arrow if there is at least one balloon).
3. Set the arrow's position at the end of the first balloon.
4. Traverse through the sorted balloons.
   - If a balloon starts after the arrow's position, we increment the number of arrows and move the arrow's position to the end of this new balloon.

### Code
```go
func findMinArrowShots(points [][]int) int {
    // Return 0 if there are no balloons
    if len(points) == 0 {
        return 0
    }

    // Sort balloons based on the end position
    sort.Slice(points, func(i, j int) bool {
        return points[i][1] < points[j][1]
    })

    // Initialize number of arrows needed
    arrows := 1
    // Initial position of the arrow at the end of the first balloon
    arrowPosition := points[0][1]

    // Traverse through all sorted balloons
    for i := 1; i < len(points); i++ {
        // If a balloon starts after the arrow's position, increase arrow count
        // And set new arrow position at the end of this balloon
        if points[i][0] > arrowPosition {
            arrows++
            arrowPosition = points[i][1]
        }
    }

    return arrows
}
```

### Time and Space Complexity
- **Time Complexity:** O(n log n), where n is the number of balloons. The sorting operation dominates this complexity.
- **Space Complexity:** O(1) if we can consider the input space as given (sort in place), otherwise O(n) due to the sorting space requirement.

This solution efficiently manages to check and account for overlaps between balloons and minimizes the number of total arrows needed to burst all balloons by strategically placing arrows at critical points—the end of the current balloons.


