# [LeetCode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approaches

1. [Greedy Approach](#greedy-approach)

---

## Greedy Approach

### Intuition:
The problem needs us to minimize the number of arrows required to burst all balloons. A logical approach here is to utilize a greedy method where we aim to burst as many balloons as possible with a single shot. 

Here's the plan: 
- We think of each balloon as an interval on a number line, representing the start and end coordinates.
- We want to "puncture" as many intervals as possible by strategically choosing a shooting point within overlapping intervals.

### Steps:
1. **Sort the Balloons by End Coordinate**: This ensures that we always attempt to "end" the process for overlapping intervals early.
2. **Initialize a Variable for Tracking Arrows**: Start counting arrows with at least one arrow because we'll definitely need one to burst the first set of balloons.
3. **Iterate Through Sorted Balloons**: Use the end of the current interval as the position for shooting and check how many subsequent balloons this arrow can cover.

### Implementation:

```python
def findMinArrowShots(points):
    if not points:
        return 0

    # Sort the points based on their end values
    points.sort(key=lambda x: x[1])

    # Start with at least one arrow
    arrows = 1
    # Set the first end point as the first arrow position
    current_end = points[0][1]

    for x_start, x_end in points:
        # If the start of the current balloon is beyond the end of current arrow's coverage
        if x_start > current_end:
            arrows += 1  # We need a new arrow
            current_end = x_end  # Update the current arrow's endpoint

    return arrows

# points represents the balloons, where each sublist is [start, end] of the balloon.
points = [[10,16],[2,8],[1,6],[7,12]]
print(findMinArrowShots(points))  # Output: 2
```

### Detailed Comments:
- **Line 1-2**: If there are no balloons, return 0 as no arrows are needed.
- **Line 5**: We sort the list of balloon intervals based on the end position to prioritize shooting from the end closest to the starting position.
- **Line 8**: Start with one arrow since any non-empty set of balloons will need at least one arrow.
- **Line 10-17**: Loop through each sorted balloon:
  - If a balloon's starting position is greater than the current end of an interval (indicating no overlap), increase the arrow count and redefine the shooting position.

### Time Complexity:
- **O(n log n)**: Dominated by the sorting step where 'n' is the number of balloons.

### Space Complexity:
- **O(1)**: Sorting is in-place, and we use a constant amount of additional space.

---

This approach capitalizes on a sorted strategy to make intelligent decisions about when and where to shoot, minimizing the total number of arrows required.

