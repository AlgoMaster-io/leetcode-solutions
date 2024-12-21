[Leetcode 452: Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

### Approaches

1. [Greedy Approach: Sorting by End Points](#greedy-approach-sorting-by-end-points)

---

#### Greedy Approach: Sorting by End Points

**Intuition:**

The key insight to solve this problem is to think about the nature of intervals. The goal is to use the minimum number of arrows, so we should attempt to burst as many overlapping balloons with a single arrow as possible. If we sort balloons by their end positions, we can iterate through each balloon, and whenever we encounter a balloon that starts after the current end of the range burst by our existing arrows, we need a new arrow.

**Steps:**

1. **Sort the balloons:** 
   Sort the array of balloons by their ending positions. This way, once an arrow bursts a group of overlapping balloons, the next arrow can start at the earliest end of any remaining balloon.

2. **Initialize arrows:**
   Start with one arrow at the end of the first balloon. Iterate over the sorted balloons.

3. **Iterate and check:**
   As you move through the sorted list, check if the start of the current balloon is greater than the position of the last burst arrow. If it is, this means a new arrow is needed. Update the position of the current arrow to the end of this new balloon.

**Time Complexity:** `O(n log n)` due to sorting.

**Space Complexity:** `O(1)` if sorting in-place, otherwise `O(n)` if we consider the space for sorting.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int findMinArrowShots(vector<vector<int>>& points) {
    // If there are no balloons, no arrows are required
    if (points.empty()) return 0;

    // Sort the balloons based on their end positions
    sort(points.begin(), points.end(), [](const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    });
    
    int arrows = 1; // At least one arrow is required
    int last_end = points[0][1]; // Place the first arrow at the end of the first balloon

    // Iterate through each balloon
    for (const auto& balloon : points) {
        // If the current balloon's start is > last_end, we need another arrow
        if (balloon[0] > last_end) {
            arrows++; // Increase the arrow count
            last_end = balloon[1]; // Move the current arrow to the end of this balloon
        }
    }
    
    return arrows; // Return the minimum number of arrows required
}
```

This approach efficiently finds the minimum number of arrows required to burst all balloons by leveraging a greedy strategy based on sorting by the end positions of the balloons. This ensures we never need more arrows than necessary.

