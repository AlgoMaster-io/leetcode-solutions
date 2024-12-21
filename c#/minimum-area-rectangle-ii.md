# [LeetCode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach Using Hash Map](#optimized-approach-using-hashmap)

---

## Brute Force Approach

### Intuition
The first approach is a brute force one where we check every possible combination of four points to see if they can form a rectangle. This involves checking if the diagonals are equal, which is a key property of rectangles. Given four points, they form a rectangle if and only if the sum of the squares of the lengths of both pairs of opposite sides are equal to the square of the length of the diagonal.

### Steps
1. Iterate over all combinations of four points using 4 nested loops.
2. For every combination of four points, calculate the squared distance between all pairs.
3. Check if those four points can form a rectangle by verifying if the sum of the squares of the lengths of both pairs of opposite sides are equal to the square of the length of the diagonal.
4. Calculate the area of the rectangle using the distance of the diagonally opposite points.
5. Keep track of the minimum area found.

### Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public double MinAreaFreeRect(int[][] points) {
        int n = points.Length;
        double minArea = double.MaxValue;
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    for (int l = k + 1; l < n; l++) {
                        // Calculate diagonal distances and square them
                        var dist1 = DistanceSquared(points[i], points[j]);
                        var dist2 = DistanceSquared(points[k], points[l]);
                        var dist3 = DistanceSquared(points[i], points[k]);
                        var dist4 = DistanceSquared(points[j], points[l]);
                        var dist5 = DistanceSquared(points[i], points[l]);
                        var dist6 = DistanceSquared(points[j], points[k]);

                        // Check if both diagonals are equal for rectangle
                        if (dist1 == dist2 && dist3 + dist4 == dist1 && dist5 + dist6 == dist2) {
                            double area = Math.Sqrt(dist1) * Math.Sqrt(dist3);
                            minArea = Math.Min(minArea, area);
                        }
                    }
                }
            }
        }
        
        return minArea == double.MaxValue ? 0 : minArea;
    }
    
    private int DistanceSquared(int[] a, int[] b) {
        return (a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]);
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n^4), where n is the number of points. This is due to the 4 nested loops iterating over the points.
- **Space Complexity**: O(1), as we are using a constant amount of extra space for calculations.

---

## Optimized Approach Using HashMap

### Intuition
The optimized approach uses the concept that given any two points that can form a diagonal of a rectangle, the two other points must lie such that they both have equal distances from the midpoint of the diagonal. This can be efficiently tracked using a hashmap where key is the midpoint and value is the list of diagonal ends.

### Steps
1. Create a hashmap to store diagonals with their midpoints as keys.
2. For each pair of points, calculate the midpoint.
3. Store each midpoint in the hashmap along with the linked pair of diagonal points.
4. For every set of pairs sharing the same midpoint, compute the potential rectangle area and update the minimum area found.

### Code
```csharp
public class Solution {
    public double MinAreaFreeRect(int[][] points) {
        int n = points.Length;
        var map = new Dictionary<(double, double), List<(int, int)> >();

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                double midx = (points[i][0] + points[j][0]) / 2.0;
                double midy = (points[i][1] + points[j][1]) / 2.0;
                
                if (!map.ContainsKey((midx, midy))) {
                    map[(midx, midy)] = new List<(int, int)>();
                }
                
                map[(midx, midy)].Add((i, j));
            }
        }

        double minArea = double.MaxValue;
        foreach (var list in map.Values) {
            int size = list.Count;
            for (int i = 0; i < size; i++) {
                for (int j = i + 1; j < size; j++) {
                    (int p1, int p2) = list[i];
                    (int q1, int q2) = list[j];

                    double area = Math.Sqrt(DistanceSquared(points[p1], points[q1])) * Math.Sqrt(DistanceSquared(points[p1], points[q2]));
                    
                    minArea = Math.Min(minArea, area);
                }
            }
        }
        
        return minArea == double.MaxValue ? 0 : minArea;
    }
    
    private int DistanceSquared(int[] a, int[] b) {
        return (a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]);
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n^2), reduced from O(n^4) as we are now processing pairs and leveraging a hashmap.
- **Space Complexity**: O(n^2), due to storage used in the hashmap for pairs of points sharing the same mid-point.

This approach reduces the computation significantly, leveraging geometric properties of a rectangle for efficiency.

