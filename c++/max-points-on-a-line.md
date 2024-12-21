# [Leetcode 149: Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Using Hash Map to Store Slopes](#using-hash-map-to-store-slopes)

### Brute Force Approach

**Intuition:**

The simplest approach to solve the problem is to check every pair of points, calculate the line equation that passes through these two points, and then count how many points lie on this line. We can represent a line with a slope and an intercept (or with a slope and a fixed point for integer calculations). By iterating through every possible combination of two points, we can calculate the maximum number of points that lie on any line defined by these point pairs. This approach, although straightforward, is computationally expensive due to its quadratic nature.

**Code:**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int n = points.size();
        if (n < 3) return n; // Handle small cases.

        int max_points = 0;

        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int count = 2; // Count the line formed by points[i] and points[j].
                for (int k = 0; k < n; ++k) {
                    if (k != i && k != j) {
                        // Check if point k lies on the line formed by point i and j
                        if ((points[j][1] - points[i][1]) * (points[k][0] - points[i][0]) ==
                            (points[k][1] - points[i][1]) * (points[j][0] - points[i][0])) {
                            count++;
                        }
                    }
                }
                max_points = max(max_points, count); // Update the max points found so far.
            }
        }
        return max_points;
    }
};
```

**Time Complexity:** O(n^3), where `n` is the number of points, as we iterate through all pairs and for each pair, we check all other points.

**Space Complexity:** O(1), since we are not using any additional data structures that scale with input size except for a few variables.

### Using Hash Map to Store Slopes

**Intuition:**

The brute force approach can be improved by using a hash map to store and count slopes of lines passing through a fixed point. For each point, consider it as the anchor and compute the slopes between the anchor and other points. By representing the slope as a reduced fraction (dy/dx), we track how many points share these slopes with the anchor point, indicating they lie on the same line. This hash map approach is more efficient since it reduces redundant calculations of checking every pair separately.

**Code:**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int n = points.size();
        if (n < 3) return n;

        int max_points = 0;

        for (int i = 0; i < n; ++i) {
            unordered_map<string, int> slope_map;
            int duplicate = 1; // Count the anchor point itself.
            int vertical = 0; // Handle vertical slopes separately.

            for (int j = i + 1; j < n; ++j) {
                int dx = points[j][0] - points[i][0];
                int dy = points[j][1] - points[i][1];

                if (dx == 0 && dy == 0) {
                    // Same point as the anchor point.
                    duplicate++;
                } else if (dx == 0) {
                    // Vertical line
                    vertical++;
                } else {
                    // Compute reduced slope dy/dx as a string key for hashmap
                    int gcd_val = gcd(dx, dy);
                    dx /= gcd_val;
                    dy /= gcd_val;

                    if (dy < 0) {
                        dx = -dx;
                        dy = -dy;
                    }

                    string slope = to_string(dy) + "/" + to_string(dx);
                    slope_map[slope]++;
                }
            }

            int current_max = vertical;
            for (auto& [slope, count] : slope_map) {
                current_max = max(current_max, count);
            }
            max_points = max(max_points, current_max + duplicate);
        }
        return max_points;
    }

private:
    int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
};
```

**Time Complexity:** O(n^2), which is more efficient than the brute force solution. For each point, we compute the line through every other point.

**Space Complexity:** O(n), where `n` represents the number of slopes (lines) recorded in the hash map for a single iteration of the outer loop.

