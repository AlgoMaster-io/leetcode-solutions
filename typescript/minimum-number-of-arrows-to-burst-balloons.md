[Leetcode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Navigation
- [Approach 1: Sorting and Greedy Approach](#approach-1-sorting-and-greedy-approach)

### Approach 1: Sorting and Greedy Approach

**Intuition:**

The key observation is that if balloons overlap, we can use one arrow to burst all overlapping balloons. For example, if you have a balloon interval [1, 6] and [2, 8], a single arrow shot between 2 and 6 will burst both.

- Sort the list of balloons by their end positions.
- Start with the end of the first balloon as the position of the first arrow.
- Figure out which balloons overlap with this set position.
- Move to the next balloon that cannot be burst by the current arrow, and set that balloon's end position as the new position for the next arrow.
- Continue this process until all balloons are burst.

This approach ensures that each arrow we use covers as many balloons as possible.

```typescript
function findMinArrowShots(points: number[][]): number {
    if (points.length === 0) return 0;

    // Sort balloons by their end position
    points.sort((a, b) => a[1] - b[1]);

    let arrows = 1;
    // Use the end of the first balloon as the position of the first arrow
    let currentEnd = points[0][1];

    for (let i = 1; i < points.length; i++) {
        // If the start of the current balloon is greater than the end of the last arrow, we need a new arrow
        if (points[i][0] > currentEnd) {
            arrows++;
            // Update currentEnd to the end of the current balloon
            currentEnd = points[i][1];
        }
    }
    
    return arrows;
}
```

**Time Complexity:**

- Sorting the balloons takes \(O(n \log n)\), where \(n\) is the number of balloons.
- Iterating through the sorted list takes \(O(n)\).
- Thus, the overall time complexity is \(O(n \log n)\).

**Space Complexity:**

- Sorting uses \(O(\log n)\) space due to the recursive nature of some sorting algorithms like quicksort.
- The algorithm maintains only a constant number of variables, so the space complexity is \(O(1)\).

