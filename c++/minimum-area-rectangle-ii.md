# [Leetcode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

## Solutions

- [Solution 1: Brute Force Quadrilateral Check](#solution-1)
- [Solution 2: Vector Property with Hashmap](#solution-2)

### Solution 1: Brute Force Quadrilateral Check
#### Intuition
The problem involves checking all possible combinations of points to see if they can form a rectangle. Given a set of points, a rectangle can be identified if:
- Opposite sides are equal and parallel.
- The diagonals are equal.

Thus, we could brute force check every combination of 4 points to see if they form a rectangle by calculating side lengths and ensuring diagonals are equal.

#### Approach
1. Generate all combinations of 4 points from the given list.
2. For each combination, calculate the distances for the sides and diagonals.
3. Verify if two opposite sides are equal and check if diagonals are equal.
4. Compute the area without needing to square root it, to keep computations within integer range.
5. Keep track of the minimum area found.

#### Code
```cpp
#include <vector>
#include <algorithm>
#include <cmath>
#include <cfloat>

using namespace std;

class Solution {
public:
    double minAreaFreeRect(vector<vector<int>>& points) {
        int n = points.size();
        double minArea = DBL_MAX;  // use double's max value as the notion of infinity

        for (int i = 0; i < n - 3; ++i) {
            for (int j = i + 1; j < n - 2; ++j) {
                for (int k = j + 1; k < n - 1; ++k) {
                    for (int l = k + 1; l < n; ++l) {
                        vector<int> p1 = points[i], p2 = points[j];
                        vector<int> p3 = points[k], p4 = points[l];

                        if (isRectangle(p1, p2, p3, p4)) {
                            // Calculate the area using vector cross product for sides
                            double area = calculateArea(p1, p2, p3);
                            minArea = min(minArea, area);
                        }
                    }
                }
            }
        }
        return minArea == DBL_MAX ? 0 : minArea;
    }

private:
    bool isRectangle(const vector<int>& p1, const vector<int>& p2, const vector<int>& p3, const vector<int>& p4) {
        // Check if diagonals p1p3 and p2p4 are equal in length
        return dist(p1, p3) == dist(p2, p4) &&
               dist(p1, p2) + dist(p3, p4) == dist(p2, p3) + dist(p1, p4);
    }

    double calculateArea(const vector<int>& p1, const vector<int>& p2, const vector<int>& p3) {
        // Area = 0.5 * |(x2 - x1)*(y3 - y1) - (x3 - x1)*(y2 - y1)|
        return abs((p2[0] - p1[0]) * (p3[1] - p1[1]) - (p3[0] - p1[0]) * (p2[1] - p1[1])) / 2.0;
    }

    int dist(const vector<int>& p1, const vector<int>& p2) {
        return (p2[0] - p1[0]) * (p2[0] - p1[0]) + (p2[1] - p1[1]) * (p2[1] - p1[1]);
    }
};
```

#### Complexity
- **Time Complexity:** O(N^4), where N is the number of points, due to iterating through all combinations of 4 points.
- **Space Complexity:** O(1), as no additional space other than variables to store distances is used.

### Solution 2: Vector Property with Hashmap
#### Intuition
To optimize, we can leverage a important property: If two diagonals are equal, then we can calculate the other two sides using vector addition/subtraction, due to the parallelogram law. If two equal diagonals exist for multiple pairs of points, and their midpoint is the same, they can form a potential rectangle.

#### Approach
1. Use a hashmap to store the midpoint and corresponding diagonal information.
2. For each pair of points, calculate the midpoint and store the diagonal squared length.
3. For each match found in the hashmap with the same midpoint and diagonal length, perform area calculation.
4. Track the minimum area.

#### Code
```cpp
#include <vector>
#include <unordered_map>
#include <cmath>
#include <cfloat>

using namespace std;

class Solution {
public:
    double minAreaFreeRect(vector<vector<int>>& points) {
        int n = points.size();
        double minArea = DBL_MAX;

        unordered_map<string, vector<pair<int, int>>> mp;

        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int x = points[i][0] + points[j][0];
                int y = points[i][1] + points[j][1];
                int d = dist(points[i], points[j]);

                string key = to_string(x) + "_" + to_string(y) + "_" + to_string(d);

                for (auto& p : mp[key]) {
                    minArea = min(minArea, sqrt(dist(points[i], points[p.first]) * dist(points[j], points[p.second])));
                }

                mp[key].emplace_back(i, j);
            }
        }
        return minArea == DBL_MAX ? 0 : minArea;
    }

private:
    int dist(const vector<int>& p1, const vector<int>& p2) {
        return (p2[0] - p1[0]) * (p2[0] - p1[0]) + (p2[1] - p1[1]) * (p2[1] - p1[1]);
    }
};
```

#### Complexity
- **Time Complexity:** O(N^2), where N is the number of points, since we check each pair of points.
- **Space Complexity:** O(N^2) due to storage in hashmap for each possible midpoint and diagonal squared length.

