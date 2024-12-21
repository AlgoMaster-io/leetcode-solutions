# [LeetCode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach with Efficient Checking](#optimized-approach-with-efficient-checking)

### Brute Force Approach

**Intuition:**

In the brute force approach, the main idea is to check every combination of four points to see if they can form a rectangle. A rectangle is formed if opposite sides are parallel and equal, and diagonals are equal. Given that there are no additional constraints provided apart from the minimum area requirement, we check all combinations.

The logic involves:
- Iterating through each combination of four points.
- Calculating distances and checking the conditions for being a rectangle.
- Updating the minimum area if a rectangle is formed.

```java
public double minAreaFreeRect(int[][] points) {
    int n = points.length;
    double minArea = Double.MAX_VALUE;

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                for (int l = k + 1; l < n; l++) {
                    // Check if (i, j, k, l) form a rectangle
                    if (isRectangle(points[i], points[j], points[k], points[l])) {
                        double area = calculateArea(points[i], points[j], points[k], points[l]);
                        minArea = Math.min(minArea, area);
                    }
                }
            }
        }
    }

    return minArea == Double.MAX_VALUE ? 0 : minArea;
}

// Check if four points form a rectangle using vector cross product check.
private boolean isRectangle(int[] p1, int[] p2, int[] p3, int[] p4) {
    return is90Degree(p1, p2, p3, p4) || is90Degree(p1, p3, p2, p4) || is90Degree(p1, p4, p2, p3);
}

// Check if the angle between two vectors is 90 degrees using dot product.
private boolean is90Degree(int[] p1, int[] p2, int[] p3, int[] p4) {
    int[] vector1 = new int[] {p2[0] - p1[0], p2[1] - p1[1]};
    int[] vector2 = new int[] {p4[0] - p3[0], p4[1] - p3[1]};
    return vector1[0] * vector2[0] + vector1[1] * vector2[1] == 0;
}

// Calculate area using vector cross product to get the absolute area enclosed.
private double calculateArea(int[] p1, int[] p2, int[] p3, int[] p4) {
    return Math.abs(crossProduct(p1, p2, p3) + crossProduct(p1, p3, p4) + crossProduct(p1, p4, p2)) / 2.0;
}

// Helper to calculate cross product.
private double crossProduct(int[] p1, int[] p2, int[] p3) {
    return (p2[0] - p1[0]) * (p3[1] - p1[1]) - (p2[1] - p1[1]) * (p3[0] - p1[0]);
}
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n^4)\), where \(n\) is the number of points. We check every combination of four points.
- **Space Complexity:** \(O(1)\), only constant extra space used.

### Optimized Approach with Efficient Checking

**Intuition:**

The brute force method, while straightforward, is inefficient due to its high complexity. To optimize, we leverage mathematical properties of vectors and key observations regarding the diagonals of a rectangle. If two given points can be diagonals of a rectangle, the middle point of the diagonal must be the same for both diagonals.

The steps involve:
- Using a map to group other points based on the middle point of potential diagonals.
- Checking for valid rectangles by verifying vector properties for the diagonals.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.HashSet;
import java.util.Set;

public double minAreaFreeRect(int[][] points) {
    int n = points.length;
    double minArea = Double.MAX_VALUE;
    Map<String, Set<String>> map = new HashMap<>();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int mx = points[i][0] + points[j][0];
            int my = points[i][1] + points[j][1];
            double distSq = distanceSquared(points[i], points[j]);

            String midPoint = mx + "," + my;
            String dist = distSq + "";

            if (!map.containsKey(midPoint)) {
                map.put(midPoint, new HashSet<>());
            }
            for (String storedDist : map.get(midPoint)) {
                if (Math.abs(Double.parseDouble(storedDist) - distSq) < 1e-9) {
                    continue; // Avoid floating point issues
                }
            }
            map.get(midPoint).add(dist);
        }
    }

    for (String key : map.keySet()) {
        Set<String> distances = map.get(key);
        if (distances.size() > 1) {
            for (String dist : distances) {
                double area = Math.sqrt(Double.parseDouble(dist) / 2) * Math.sqrt(Double.parseDouble(dist) / 2);
                minArea = Math.min(minArea, area);
            }
        }
    }

    return minArea == Double.MAX_VALUE ? 0 : minArea;
}

// Calculate the squared distance to avoid floating point issues
private double distanceSquared(int[] p1, int[] p2) {
    int dx = p2[0] - p1[0];
    int dy = p2[1] - p1[1];
    return dx * dx + dy * dy;
}
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n^2)\), because we process each pair of points and use a map to store distances.
- **Space Complexity:** \(O(n^2)\), due to storage in the hash maps for diagonal information.

In this statement, we take advantage of the properties of the diagonals of rectangles that show that two diagonals meet at a midpoint. By grouping diagonals based on midpoints and respective vector attributes, we can considerably improve upon the brute force approach.

