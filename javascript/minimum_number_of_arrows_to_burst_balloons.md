# 452. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approach: Greedy with Sorting

### Solution
```javascript
// Time Complexity: O(n * log(n)), where n is the number of balloons (due to sorting)
// Space Complexity: O(1), ignoring the input storage

function findMinArrowShots(points) {
    if (!points || points.length === 0) {
        return 0;
    }

    // Step 1: Sort the balloons by their end points
    points.sort((a, b) => a[1] - b[1]);

    let arrows = 1; // At least one arrow is needed
    let end = points[0][1]; // The end of the first balloon

    // Step 2: Iterate through the balloons
    for (let i = 1; i < points.length; i++) {
        if (points[i][0] > end) {
            // If the current balloon starts after the previous end, we need a new arrow
            arrows++;
            end = points[i][1]; // Update the end to the current balloon's end
        }
    }

    return arrows;
}
```

