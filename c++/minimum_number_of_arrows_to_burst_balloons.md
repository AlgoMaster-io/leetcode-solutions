# 452. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

## Approach: Greedy with Sorting

### Solution
c++
// Time Complexity: O(n * log(n)), where n is the number of balloons (due to sorting)
// Space Complexity: O(1), ignoring the input storage
#include <vector>
#include <algorithm>

class Solution {
public:
    int findMinArrowShots(std::vector<std::vector<int>>& points) {
        if (points.empty()) {
            return 0;
        }

        // Step 1: Sort the balloons by their end points
        std::sort(points.begin(), points.end(), [](const std::vector<int>& a, const std::vector<int>& b) {
            return a[1] < b[1];
        });

        int arrows = 1; // At least one arrow is needed
        int end = points[0][1]; // The end of the first balloon

        // Step 2: Iterate through the balloons
        for (size_t i = 1; i < points.size(); i++) {
            if (points[i][0] > end) {
                // If the current balloon starts after the previous end, we need a new arrow
                arrows++;
                end = points[i][1]; // Update the end to the current balloon's end
            }
        }

        return arrows;
    }
};

