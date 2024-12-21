# [149. Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

## Solutions
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Hashmaps with Rational Number for Slopes](#approach-2-using-hashmaps-with-rational-number-for-slopes)
- [Approach 3: Using Hashmaps with Simplified Slope Representation](#approach-3-using-hashmaps-with-simplified-slope-representation)

### Approach 1: Brute Force

#### Intuition:
The simplest approach is to consider every pair of points and determine the line they form. Once the line formed by a pair of points is known, count how many points lie on this line by checking the collinearity condition.

For two points (x1, y1) and (x2, y2), the slope is (y2 - y1) / (x2 - x1). Two points (x3, y3) and (x1, y1) are collinear with this line if (y3 - y1) / (x3 - x1) equals (y2 - y1) / (x2 - x1).

#### Detailed Steps:
1. Initialize a variable `max_points` to store the maximum number of points on a line.
2. For each pair of points, calculate the line they form.
3. For each other point, check if it is collinear with the selected line.
4. Update `max_points` if the current line has more points.
5. Return `max_points`.

#### Code:
```python
from collections import defaultdict

def maxPoints(points):
    def gcd(a, b):
        while b:
            a, b = b, a % b
        return a

    if not points:
        return 0

    n = len(points)
    if n == 1:
        return 1

    max_points = 0

    for i in range(n):
        slopes = defaultdict(int)
        same_point_count = 1
        local_max = 0

        for j in range(i + 1, n):
            dx = points[j][0] - points[i][0]
            dy = points[j][1] - points[i][1]

            if dx == 0 and dy == 0:
                same_point_count += 1
                continue

            gcd_of_slope = gcd(dx, dy)
            dx //= gcd_of_slope
            dy //= gcd_of_slope

            slopes[(dy, dx)] += 1
            local_max = max(local_max, slopes[(dy, dx)])

        max_points = max(max_points, local_max + same_point_count)

    return max_points
```

#### Complexity Analysis:
- **Time Complexity:** O(n^2), where n is the number of points. We check all pairs of points.
- **Space Complexity:** O(n) for storing slopes in the hash map.

### Approach 2: Using Hashmaps with Rational Number for Slopes

#### Intuition:
Instead of iterating over all pairs of points, for each point, consider it as an anchor and calculate slopes with every other point. Use a hashmap to count points with the same slope, as this will determine how many points lie on the same line.

#### Detailed Steps:
1. Iterate through each point and treat it as the anchor point.
2. For each other point, calculate the slope and store it in a hashmap.
3. For same starting points, track the count separately.
4. Calculate the maximum number of points on a line passing through the anchor point.
5. Update global maximum accordingly.

#### Code:
```python
def maxPoints(points):
    def gcd(a, b):
        while b:
            a, b = b, a % b
        return a

    max_points = 0

    for i in range(len(points)):
        slopes = defaultdict(int)
        same_point_count = 1
        local_max = 0

        for j in range(i + 1, len(points)):
            dx = points[j][0] - points[i][0]
            dy = points[j][1] - points[i][1]

            if dx == 0 and dy == 0:
                same_point_count += 1
                continue

            gcd_of_slope = gcd(dx, dy)
            dx //= gcd_of_slope
            dy //= gcd_of_slope

            slopes[(dy, dx)] += 1
            local_max = max(local_max, slopes[(dy, dx)])
        
        max_points = max(max_points, local_max + same_point_count)

    return max_points
```

#### Complexity Analysis:
- **Time Complexity:** O(n^2), for iterating through all points while computing slopes.
- **Space Complexity:** O(n), utilizing the hashmap for slopes.

### Approach 3: Using Hashmaps with Simplified Slope Representation

#### Intuition:
Utilize the simplified slope form to avoid precision issues encountered with floating point division by using integer pair representation along with their GCD.

#### Detailed Steps:
1. Iterate through each point using it as an anchor point.
2. Use integer division to calculate slope considering GCD to simplify slope representation.
3. Use hashmap to store frequency of each slope.
4. Consider all variations including vertical lines and overlapping points.
5. Determine the maximum number of collinear points and update global maximum.

#### Code:
```python
def maxPoints(points):
    from math import gcd
    max_points = 0
    n = len(points)

    for i in range(n):
        slopes = defaultdict(int)
        duplicate = 1
        local_max = 0

        for j in range(i + 1, n):
            x_diff = points[j][0] - points[i][0]
            y_diff = points[j][1] - points[i][1]

            if x_diff == 0 and y_diff == 0:
                duplicate += 1
                continue

            g = gcd(x_diff, y_diff)
            x_diff //= g
            y_diff //= g

            if x_diff == 0:
                y_diff = 1
            elif y_diff == 0:
                x_diff = 1
            elif x_diff < 0:
                x_diff, y_diff = -x_diff, -y_diff

            slopes[(x_diff, y_diff)] += 1
            local_max = max(local_max, slopes[(x_diff, y_diff)])

        max_points = max(max_points, local_max + duplicate)

    return max_points
```

#### Complexity Analysis:
- **Time Complexity:** O(n^2), due to iterating through all point pairs while calculating slopes.
- **Space Complexity:** O(n), due to storing slopes in a hashmap.

