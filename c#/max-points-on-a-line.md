# [Max Points on a Line - LeetCode](https://leetcode.com/problems/max-points-on-a-line/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using HashMap to find Slopes Between Every Pair of Points](#approach-2-using-hashmap-to-find-slopes-between-every-pair-of-points)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach involves examining each possible pair of points `(i, j)` and determining if other points lie on the line passing through them. For every chosen pair of points we consider, we will count the number of points that have the same slope with these given points.

### Code:
```csharp
public class Solution {
    public int MaxPoints(int[][] points) {
        if (points.Length < 2) return points.Length;

        int maxPoints = 1;

        for (int i = 0; i < points.Length; i++) {
            for (int j = i + 1; j < points.Length; j++) {
                
                // Start with the original pair of points on the line
                int count = 2;
                for (int k = 0; k < points.Length; k++) {
                    if (k == i || k == j) continue;

                    // Check if point k is on the line formed by i and j
                    if ((points[j][1] - points[i][1]) * (points[k][0] - points[i][0]) ==
                        (points[k][1] - points[i][1]) * (points[j][0] - points[i][0])) {
                        count++;
                    }
                }
                
                maxPoints = Math.Max(maxPoints, count);
            }
        }
        return maxPoints;
    }
}
```
### Complexity:
- **Time Complexity:** \(O(n^3)\), where \(n\) is the number of points, as we are checking every possible pair and then every other point for collinearity.
- **Space Complexity:** \(O(1)\), as no additional space is being used.

---

## Approach 2: Using HashMap to find Slopes Between Every Pair of Points

### Intuition:
Instead of recalculating for each pair, use a hashmap to store the slope of the line formed by a point and other points. The slope is calculated for each base point and then determine the maximum number of points that have the same slope from this base point.

### Code:
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int MaxPoints(int[][] points) {
        if (points.Length < 2) return points.Length;

        int maxPoints = 1;

        for (int i = 0; i < points.Length; i++) {
            // Dictionary to store the slope and corresponding count of points with that slope
            Dictionary<(int, int), int> slopeCount = new Dictionary<(int, int), int>();
            int samePoint = 0;
            int maxPointsForCurrent = 0;

            for (int j = i + 1; j < points.Length; j++) {
                int dx = points[j][0] - points[i][0];
                int dy = points[j][1] - points[i][1];

                if (dx == 0 && dy == 0) {
                    samePoint++;
                    continue;
                }

                // Reduce the slope to a normalized form
                int gcd = GCD(dx, dy);
                dx /= gcd;
                dy /= gcd;

                // Store the reduced form in the HashMap
                var slope = (dx, dy);
                
                if (slopeCount.ContainsKey(slope)) {
                    slopeCount[slope]++;
                } else {
                    slopeCount[slope] = 1;
                }

                maxPointsForCurrent = Math.Max(maxPointsForCurrent, slopeCount[slope]);
            }
            
            // Include the same points and the point itself
            maxPoints = Math.Max(maxPoints, maxPointsForCurrent + samePoint + 1);
        }
        
        return maxPoints;
    }
    
    // Helper function to find Greatest Common Divisor
    private int GCD(int a, int b) {
        if (b == 0) return a;
        return GCD(b, a % b);
    }
}
```

### Complexity:
- **Time Complexity:** \(O(n^2)\), where \(n\) is the number of points. For each point, we compute the slope with every other point.
- **Space Complexity:** \(O(n)\), for storing the slopes in the hashmap.

