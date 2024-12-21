# [Leetcode 149: Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using HashMap](#optimized-approach-using-hashmap)

---

## Brute Force Approach

### Intuition
This approach considers all combinations of pairs to determine every possible line. For each line, we check how many points from the input lie on it.

**Steps:**
1. Iterate over each pair of points.
2. For each pair, calculate the slope.
3. Count how many points lie on the line defined by this slope.
4. Keep track of the maximum count.

### Implementation

```java
public class Solution {
    public int maxPoints(int[][] points) {
        if (points.length <= 2) return points.length;
        
        int maxPointsOnLine = 0;
        
        // Consider every pair of points
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                int count = 2; // We already have two points, i and j
                for (int k = 0; k < points.length; k++) {
                    if (k == i || k == j) continue;
                    
                    // Check if the point k is on the line formed by i and j
                    if ((points[j][0] - points[i][0]) * (points[k][1] - points[i][1]) ==
                        (points[j][1] - points[i][1]) * (points[k][0] - points[i][0])) {
                        count++;
                    }
                }
                maxPointsOnLine = Math.max(maxPointsOnLine, count);
            }
        }
        
        return maxPointsOnLine;
    }
}
```

### Complexity
- **Time Complexity**: O(N^3), where N is the number of points. This is due to the triply nested loops.
- **Space Complexity**: O(1), no extra space is used.

---

## Optimized Approach Using HashMap

### Intuition
Instead of evaluating every pair by brute force, leverage the properties of the slope of lines and use a HashMap to optimize the counting of points on a similar slope from a given point.

**Steps:**
1. Iterate over each point and treat it as the base point.
2. Use a HashMap to store the slope of lines that include the base point.
3. For each other point, calculate the simplified form of the slope and use it as the key in the HashMap.
4. Determine the highest count of points that lie on any line based from the current base point.
5. Keep track of the maximum count across all base points.

### Implementation

```java
import java.util.HashMap;

public class Solution {
    public int maxPoints(int[][] points) {
        if (points.length <= 2) return points.length;
        
        int maxPointsOnLine = 0;
        
        for (int i = 0; i < points.length; i++) {
            HashMap<String, Integer> slopes = new HashMap<>();
            int duplicate = 1; // It accounts for the base point itself
            int curMax = 0;
            
            for (int j = 0; j < points.length; j++) {
                if (i == j) continue;
                
                int deltaY = points[j][1] - points[i][1];
                int deltaX = points[j][0] - points[i][0];
                
                if (deltaY == 0 && deltaX == 0) {
                    // The current point is a duplicate of the base point
                    duplicate++;
                    continue;
                }
                
                // Simplify the slope by dividing by the greatest common divisor
                int gcd = gcd(deltaY, deltaX);
                deltaY /= gcd;
                deltaX /= gcd;
                
                // Ensure a consistent representation of the slope (dy between a line of identical x coordinates)
                String slopeKey = deltaY + "/" + deltaX;
                slopes.put(slopeKey, slopes.getOrDefault(slopeKey, 0) + 1);
                
                curMax = Math.max(curMax, slopes.get(slopeKey));
            }
            
            maxPointsOnLine = Math.max(maxPointsOnLine, curMax + duplicate);
        }
        
        return maxPointsOnLine;
    }
    
    // Helper function to find greatest common divisor
    private int gcd(int a, int b) {
        if (b == 0) return a;
        return gcd(b, a % b);
    }
}
```

### Complexity
- **Time Complexity**: O(N^2), because we iterate through each point and calculate the slope against every other point.
- **Space Complexity**: O(N), because we use a HashMap to store slopes information for each base point.

