# 452. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approach: Greedy with Sorting

### Solution
go
```go
// Time Complexity: O(n * log(n)), where n is the number of balloons (due to sorting)
// Space Complexity: O(1), ignoring the input storage
import "sort"

func findMinArrowShots(points [][]int) int {
    if len(points) == 0 {
        return 0
    }

    // Step 1: Sort the balloons by their end points
    sort.Slice(points, func(i, j int) bool {
        return points[i][1] < points[j][1]
    })

    arrows := 1 // At least one arrow is needed
    end := points[0][1] // The end of the first balloon

    // Step 2: Iterate through the balloons
    for i := 1; i < len(points); i++ {
        if points[i][0] > end {
            // If the current balloon starts after the previous end, we need a new arrow
            arrows++
            end = points[i][1] // Update the end to the current balloon's end
        }
    }

    return arrows
}
```

